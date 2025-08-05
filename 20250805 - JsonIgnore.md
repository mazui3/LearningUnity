用于class中一些属性json化的时候不希望被带上。
>https://learn.microsoft.com/zh-tw/dotnet/standard/serialization/system-text-json/ignore-properties
>https://stackoverflow.com/questions/58228555/what-is-use-of-the-annotation-jsonignore

```
class User {
    @JsonIgnore
    private int id;
    private String name;
    public int getId() {
        return id;
    }
    @JsonIgnore
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }  
}
```

Sample code for serialization:
```
ObjectMapper mapper = new ObjectMapper();
User user = new User();
user.setId(2);
user.setName("Bob");
System.out.println(mapper.writeValueAsString(user));
```

Console output:
>{"name":"Bob"}
