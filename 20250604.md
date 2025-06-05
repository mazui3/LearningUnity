BinaryFormatter
MemoryStream
binaryFormatter.Serialize(memoryStream,theClass)

memoryStream.Position = 0;

生成BinaryFormatter然后serialize.
可以把数据class变成binary,然后做成deep copy.
要求format class得是serializable,monobahaviour做不到.
