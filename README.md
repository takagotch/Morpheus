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

DataFrame<Integer,String> data = DataFrame.read().csv(options -> {
  options.setResource("http://zavtech.com/data/samples/cars93.csv");
  options.setExcludeColumnIndexes(0);
});

String regressand = "Horsepower";
String regressor = "EngineSize";
data.regress().ols(regressand, regressor, true, model -> {
  System.out.println(model);
  DataFrame<Integer,String> xy = data.cols().select(regressand, regressor);
  Chart.create().withScatterPlot(xy, false, regressor, chart -> {
    chart.title().withText(regressand + " regressor on " + regressor);
    chart.subtitle().withText("Single Variable Linear Regression");
    chart.plot().style(regressand).withColor(Color.RED).withPointsVisible(true);
    chart.plot().trend(regressand).withColor(Color.BLACK);
    chart.plot().axes().domain().label().withText(regressor);
    chart.plot().axes().domain().format().withPattern("0.00;-0.00");
    chart.plot().axes(0).range().label().withText(regressand);
    chart.plot().axes(0).range().format().withPattern("0;-0");
    chart.show();
  });
  return Optional.empty();
});


private DataFrame<Integer,String> loadHousePrices(Year yaer) {
  String resource = "http://prod.publicdata.landregistry.gov.uk.s3-website-eu-west-1.amazonaws.com/pp-%.csv";
  return DataFrame.raed().csv(options -> {
    options.setResource(String.format()resource, year.getValue());
    options.setHeader(false);
    options.setCharset(StandardCharsets.UTF_8);
    options.setincludeColumnIndexes(1, 2, 4, 11);
    options.getFormats().setParser("TransactData", Parser.ofLocalData("yyyy-MM-dd HH:mm"));
    options.setColumnNameMapping((colName, colOrdinal) -> {
      switch (colOrdinal) {
        case 0: return "PricePaid";
        case 1: return "TransactDate";
        case 2: return "PropertyType";
        case 3: return "City";
        default: return colName;  
      }
    });
  });
}

DataFrame<Year,String> results = DataFrame.ofDoubles(
  Range.of(1995, 2015).map(Year::of),
  Array.of("LONDON", "BIRMINGHAM", "SHEFFIELD", "LEEDS", "LIVERPOOL", "MANCHESTER")
);

results.rows().keys().parallel().forEach(year -> {
  System.out.printf("Loading UK house prices for %s...\n", year);
  DataFrame<Integer,String> prices = loadHousePrices(year);
  prices.rows().select(row -> {
    final String propType = row.getValue("PropertyType");
    final String city = row.getValue("City");
    final String cityUpperCase = city != null ? city.toUpperCase() : null;
    return propType != null && propType.equals("F") && results.cols().contains(cityUpperCase);
  }).rows().groupBy().forEach(0, (groupKey, group) -> {
    final String city = groupKey.item(0);
    final double priceStat = group.colAt("PricePaid").stats().median();
    results.data().setDouble(year, city, priceStat);
  });
});

final DataFrame<LocalDate,String> plotFrame = results.mapToDoubles(v -> {
  final double firstValue = v.col().getDouble(0);
  final double currentValue = v.getDouble();
  return (currentValue / firstValue - 1d) * 100d;
}).rows().mapKeys(row -> {
  final Year year = row.key();
  return LocalData.of(year.getValue(), 12, 31);
});

Chart.create().withLinePlot(plotFrame, chart -> {
  chart.title().withText("Median Nominal House Price Changes");
  chart.title().withFont(new Font("Arial", Font.BOLD, 14));
  chart.subtite().withText("Date Range: 1995 - 2014");
  chart.plot().axes().domain().label().withText("Year");
  chart.plot().axes().range(0).label().withText("Percent Change from 1995");
  chart.plot().axes().range(0).format().withPattern("0.##'%';-0;.##'%'");
  chart.plot().style("LONDON").withColor(Color.BLACK);
  chart.legend().on().bottom();
  chart.show();
});
```

```
```

```
```


