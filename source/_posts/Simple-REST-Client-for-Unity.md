---
title: Simple REST Client for Unity
date: 2015-04-22  23:50:17
tags: 
- NET
- REST Client
- API
- unity3d
- unity
- C#
category: Game Development
---



# Simple REST Utility for Unity 


### History:

Recently I needed a script though which I could easily get and send data to a REST Server. I picked my fav environment: MEAN. using Yeoman, I quickly implemented a simplistic REST Server, which will serve all data for my Unity WebPlayer Build. So here goes my take on a simple REST Utility for Unity.

<!-- more -->


### NOTE:
Since UNity 5.3, the in-built REST client, ```httpwebrequest``` is really good and hence, you won't be needing this tool at all. For below versions, you can use this code.



Unity has the wonderful WWW utility through which we can GET or POST data to/from any REST Server. After stumbling upon further, I found a [StackOverflow](http://stackoverflow.com/questions/8951489/unity-get-post-wrapper) post for a Simple GET/POST Wrapper for REST Utility for Unity.

I tweaked it a little ( added Callbacks instead of directly returning values) and use din my projects and it worked like a charm. And yes, for those who were concerned if it will work for WebBuilds, well, Yes, it works. I have Tested it.
The following is the code:

### LIBRARY CODE
```c#
// file : DB.cs ( put this in Plugins Folder)


using System;
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using Object = System.Object;

/// <summary>
/// Class D3VRest. A Wrapper class for simple and efficient REST API calls
/// </summary>

namespace D3VRestWrapper
{
    class D3VRest : MonoBehaviour
    {
        void Start() { }

        /// <summary>
        /// Gets JSON data from the specified URL.
        /// </summary>
        /// <param name="url">The URL.</param>
        /// <param name="Callback">The callback.</param>
        public void GET(string url, System.Action<Object> Callback = null)
        {
            WWW www = new WWW(url);
            StartCoroutine(WaitForRequest(www, Callback));
        }

        /// <summary>
        /// Gets a texture form the specified URL.
        /// </summary>
        /// <param name="url">The URL.</param>
        /// <param name="Callback">The callback.</param>
        public void GETTexture(string url, System.Action<Object> Callback = null)
        {
            WWW www = new WWW(url);
            StartCoroutine(WaitForRequest(www, Callback, true));
        }

        /// <summary>
        /// GETs an asset from Asset Bundle
        /// </summary>
        /// <param name="url">The Url to fetch the Asset from</param>
        /// <param name="AssetName">Asset Name</param>
        /// <param name="Callback">The Callback that contains the GameObject as its parameter</param>
        public void GETAsset(string url, string AssetName, System.Action<Object> Callback = null)
        {
            StartCoroutine(WaitForAsset(url, AssetName, Callback));
        }


        /// <summary>
        /// Posts a Form data to the specified URL.
        /// </summary>
        /// <param name="url">The URL.</param>
        /// <param name="post">The post.</param>
        /// <param name="Callback">The callback.</param>
        /// <returns>UnityEngine.WWW.</returns>
        public WWW POST(string url, Dictionary<string, string> post, System.Action<Object> Callback = null)
        {
            WWWForm form = new WWWForm();
            foreach (KeyValuePair<String, String> post_arg in post)
            {
                form.AddField(post_arg.Key, post_arg.Value);
            }
            WWW www = new WWW(url, form);

            StartCoroutine(WaitForRequest(www, Callback));
            return www;
        }



        /// <summary>
        /// Waits for request.
        /// </summary>
        /// <param name="www">The WWW.</param>
        /// <param name="Callback">The callback.</param>
        /// <param name="isTexture">This is texture or not.</param>
        /// <returns>System.Collections.IEnumerator.</returns>
        private IEnumerator WaitForRequest(WWW www, System.Action<System.Object> Callback = null, bool isTexture = false)
        {
            yield return www;
            Object r;
            if (www.error == null)
            {
                //Ok
                if (!isTexture)
                    r = "{err:false,data:" + www.text + "}";
                else
                    r = www.texture;

                if (Callback != null)
                    Callback(r);
            }
            else
            {
                //send error
                r = "{err:\"" + www.error + "\",data:false}";
                if (Callback != null)
                    Callback(r);
            }
        }

        /// <summary>
        /// Generic Asset Downloader ( Non Cached Version)
        /// </summary>
        /// <returns>System.Object as Parameter to callback</returns>
        private IEnumerator WaitForAsset(string BundleURL, string AssetName, System.Action<System.Object> Callback = null, bool isTexture = false)
        {
            //  string BundleURL = "localhost:3000/prefabs/t1.unity3d";
            // string AssetName = "t1";


            using (WWW www = new WWW(BundleURL))
            {
                yield return www;
                AssetBundle bundle;


                if (www.error == null)
                {
                    //Ok
                    bundle = www.assetBundle;

                    if (Callback != null)
                    {
                        if (AssetName == "")
                            Callback(bundle.mainAsset);
                        else
                            Callback(bundle.Load(AssetName));
                    }
                    bundle.Unload(false);
                }
                else
                {
                    //send error
                    Object r = "{err:\"" + www.error + "\",data:false}";
                    if (Callback != null)
                        Callback(r);
                }


            } // memory is freed from the web stream (www.Dispose() gets called implicitly)
        }

    }



```


### Example USAGE CODE
For example, create a file and fill it  up with this:
```C#

using System;
using System.Collections;
using System.Collections.Generic;
using SimpleJSON;
using Object = System.Object;
using D3VRestWrapper;
 
public class main : MonoBehaviour
{
    D3VRest db;
    Texture2D boxTex;
    GameObject box;
    // Use this for initialization
    void Start()
    {
        db = GameObject.Find("DB").GetComponent<D3VRest>();
        FetchJackpot();
 
 
        FetchTexure();
 
 
       
    }
 
    /// <summary>
    /// Fetches the texure.
    /// </summary>
    public void FetchTexure()
    {
        db.GETTexture("http://localhost:3000/textures/playBtn.png", delegate(object data) {
            boxTex=data as Texture2D;
            GameObject.Find("Quad").renderer.material.mainTexture=boxTex;
 
            FetchT();
        });
    }
 
    /// <summary>
    /// Fetches the jackpot.
    /// </summary>
    public void FetchJackpot()
    {
        db.GET("http://localhost:3000/jackpot", delegate(Object data)
        {
            //Data has been Received. Lets convert the returned object to JSON data
            var www = JSON.Parse(data.ToString());
 
            //CHeck if Error is present or not. ( and hopefully not!)
            if (!www["err"].AsBool)
            {
                //Check if data is present or not
                if (www["data"] != null && www["data"]["score"] != null)
                {
                    print("Score: " + www["data"]["score"].Value);
                    GameObject.Find("lblJackpot").GetComponent<UILabel>().text = "Jackpot: " + www["data"]["score"].Value;
                }
                else
                    Debug.LogWarning("Score: 0");
            }
            else
            {
                Debug.LogError("Error Occured! : " + www["err"].ToString());
            }
        });
    }
 
 
    /// <summary>
    /// Updates the jackpot.
    /// </summary>
    public void UpdateJackpot()
    {
        //Prepare Data to be Sent
        Dictionary<string, string> form = new Dictionary<string, string>();
        form.Add("score", GameObject.Find("inpJackpot").GetComponent<UIInput>().value);
 
 
        //Send the data
        db.POST("http://localhost:3000/jackpot", form, delegate(Object data)
        {
            var www = JSON.Parse(data.ToString());
            if (!www["err"].AsBool)
            {
                FetchJackpot();
                Debug.Log("Score Updated!");
            }
            else
            {
                Debug.LogError("Error Occured! : " + www["err"].ToString());
            }
        });
    }
 
 
    public void FetchT()
    {
        db.GETAsset("localhost:3000/prefabs/t1.unity3d", "t1", delegate(Object data) {
            box = Instantiate((UnityEngine.Object)data) as GameObject;
            box.renderer.material.mainTexture = boxTex;
        });
    }
 
   
 
}

```



Genrally, this simple REST utility for unity could be enhanced more and JSON can be integrated as well. I will leave this upto you. Or may be I will find out time to add more features to it and add it to my very own Utility Library ( _unity).


