ServiceLocator
==============

.. highlight:: csharp

A (somewhat) simple implementation of the service locator pattern.
The **ServiceLocator** knows about **MonoBehaviours** and how to work with them.
Creating scene-specific resolvers that only live as long as their respective scene is also supported.

Singleton
---------
Whenever a singleton type is resolved the same instance is always returned. 

Lazyloading
~~~~~~~~~~~
You can also choose to defer the creation of a singleton instance until the instance is first looked up. You can do this by simply using the overloads with 'Lazy' in their name.

Transient
---------
If you register a type as transient, a new instance of that type will be created each time it is resolved. 

If you for some reason need to work with transient **MonoBehaviours** (Please tell me why, I'm honestly very curious), you should remember to destroy them yourself again when no longer used.

Scene-specific resolvers
------------------------
Both singleton and transient resolvers can be set as scene-specific, so they only live as long as the scene they're in. This works for both **MonoBehaviours** and regular objects.

Active scene as defined by Unity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Please note that when you load a new scene, the **MonoBehaviours** in that scene will have their *Awake()* method called before their scene becomes the active one. This means you can't rely on *SceneManager.GetActiveScene()* to return the scene they're in, so you might want to use *GameObject.scene* to specify which scene to register the resolver for.

Usage
-----
.. todo::
    To be done.

Examples
--------
Singleton
~~~~~~~~~
::

    interface IStore
    {
        bool Purchase(string sku);
    }

    class AppleIAP : IStore
    {
        public bool Purchase(string sku)
        {
            // buy the item using the App Store API
        }
    }

    class GoogleIAP : IStore
    {
        public bool Purchase(string sku)
        {
            // buy the item using Google's In App Billing API
        }
    }

    class NullStore : IStore
    {
        public bool Purchase(string sku)
        {
            // do nothing or maybe log the action for debugging purposes
        }
    }

    // At game startup:
    class Game
    {
        void Initialize()
        {
    #if UNITY_ANDROID
            ServiceLocator.RegisterSingleton<IStore, GoogleIAP>();
    #elif UNITY_IOS
            ServiceLocator.RegisterSingleton<IStore, AppleIAP>();
    #else
            ServiceLocator.RegisterSingleton<IStore, NullStore>();
    #endif
        }
    }

    // An "Ad Removal" purchase button:
    class BuyButton
    {
        public void OnPressed()
        {
            var success = ServiceLocator.Resolve<IStore>().Purchase("AdRemoval");
            if (success)
            {
                PlaySound("ka-ching.ogg");
                AdManager.Enabled = false;
            }
        }
    }

Transient
~~~~~~~~~
.. todo::
    To be done.

Scene-specific singletons
~~~~~~~~~~~~~~~~~~~~~~~~~
.. todo::
    To be done.
