[top]

### HashMap
```Java
Map<String, String> mapData = new HashMap<String, String>();
// get
mapData.get("mykey");
// values
mapData.values();
// put
mapData.put("mykey", "mystring");
```

### Collection
```Java
Collection<String> c = new ArrayList<String>();
// add
c.add("mystring");
// length
c.size();
```

### for each
```Java
for (String s: mapData.values()){

}
```

### HTTP
```Java
URL url = new URL("http://...");
InputStreamReader isr = new InputStreamReader(url.openStream());
BufferedReader reader = new BufferedReader(isr);
reader.readline();
reader.close();
```

### SUM
```Java
int[] scoreArray = new int[]{1, 2, 3};
int sumScore = Arrays.stream(scoreArray).sum();
```