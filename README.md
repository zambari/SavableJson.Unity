# SavableJson for  Unity3D

SavableJson is a shortcut method for creating persistent singleton data structures. It simplifies the process of finding a serialized form on drive, loading it, and saving changes. 

I know singletons are risky but how many configs does your app/game need?

The main rules are:
If it fails to load config file, it will create a new one and write in to disk to persistendatapath, so that user can find it and modify to its needs - no need to distribute default config files, they will be creted. Instance is guaranteed to exitst.

Static .Save() method writes data to disk. If you call it on OnDestory, any changes to your config will be saved on exit.

## Usage

Create your POCO data representation, inheriting from SavableJson, parametrizing it with your class, example below:

You need to override a filename string providing a desired .json filename. It will be placed in PersistentDataPath specific to your platform

```
public class OSCConfig : SavableJson<OSCConfig>
{
    public override string fileName => "oscConfig.json";
    public int targetPort=9999;
}

```
Use example. The following class will report and increased number with each run. File will be saved on drive.
```
public class OSCConfigLoader : MonoBehaviour
{
  
    void Start()
    {
         OSCConfig.instance.targetPort++;
    }
    
    void OnDestroy()
    {
        OSCConfig.Save();
    }
}

```

## Notes

Much more can be done but it fits my everyday needs of quickly storing some config and making it user editable

If you need a constructor, override Prepare() method. There was some issue with constructors I can't remember now.

