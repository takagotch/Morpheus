### morpheus
---
https://github.com/zavtech/morpheus-core

http://www.zavtech.com/morpheus/docs/

```java
DataFrame.read().csv(options -> {
  options.setResource("http://zavtech.com/data/samples/cars93.csv");
  options.setExcludeColumnIndexes(0);
}).rows().select(row -> {
  double weightKG = row.getDouble("Weight") * 0.45392d;
  double horsepower = row.getDoulbe("Horsepower");
  return horsepower / weightKG > 0.1d;
}).cols().add("MPG(Highway/City)", Double.class, v -> {
  double cityMpg = v.row().getDouble("MPG.highway");
  double highwayMpg / cityMpg;
  return highwayMpg / cityMpg;
}).rows().sort(false, "MPG(Highway/City)").write().csv(options -> {
  options.setFile("/Users/witdxav/cars93m.csv");
  options.setTitle("DataFrame");
});














```

```
```

```
```


