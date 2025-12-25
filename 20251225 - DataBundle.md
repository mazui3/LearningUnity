Data Bundle是班码里的一个结构,真好用啊…

主要思想是这样的,我们会处理不同的PiecesEvent.\
PiecesEvent也作为一种key,在那个event中所有需要存储的信息全部丢到一个dictionary里面,不需要统一key.\
这个dictionary就是data bundle.

```cs
public class DataBundle
{
  private static Stack<DataBundle> m_defaultBundle = new Stack<DataBundle>();
  private Dictionary<string,object> m_dicParams;
  public static DataBundle getDefaultBundle()
  {
      if(m_defaultBundle.Count<=0)
      {
          DataBundle bd = new DataBundle();
          return bd;
      }
      
     return m_defaultBundle.Pop();
  }

  private DataBundle()
  {
    m_dicParams = new Dictionary<string,object> ();
  }

  public bool haskey(string key)
  {
      return m_dicParams.ContainsKey(key);
  }
  public void removekey(string key)
  {
    if(m_dicParams.ContainsKey(key))
    {
        m_dicParams.Remove(key);
    }
  }

  public DataBundle setValue<T>(string key,T value)
  {
    if(m_dicParams.ContainsKey(key))
    {
      Debug.LogError ("重复的key值");
      return this;
    }
    m_dicParams.Add (key,value as object);
    return this;
  }

    public T getValue<T>(string key,T defaultvalue = default(T))
		{
			if(!m_dicParams.ContainsKey(key))
			{
				Debug.Log ("没有key值为："+key+"的相关内容");
				return defaultvalue;
			}
			object data = m_dicParams[key];

			return (T)data;
		}
    public void copyData(Dictionary<string, object> dest)
    {
      foreach(KeyValuePair<string,object> kvp in m_dicParams)
      {
          dest.Add(kvp.Key, kvp.Value);
      }
    }

    private void clearData()
    {
      m_dicParams.Clear();
    }
    public void recovery()
    {
      clearData();
      m_defaultBundle.Push(this);
    }

}
```
