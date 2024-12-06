# Fury Benchmarks

Fury benchmarks with:
- [x] [jackson databind](https://github.com/FasterXML/jackson-databind)
- [x] [Microstream](https://github.com/real-logic/simple-binary-encoding)

# Benchmark Setup (2024)
JMH config:
`-f 3 -wi 5 -i 5 -t 1 -w 2s -r 2s -rf csv`
OS: Windows 11
Hardware: 12th Gen Intel Core i7-12800H at 2.40 GHz (laptop)
JDK: Java 23, Adoptium OpenJDK

# All Benchmark Results

```
Benchmark                                         Mode  Cnt         Score         Error  Units
AvajeBenchmark.avajeDeserializeMediaContent      thrpt   15   1238041.118 ±   39720.414  ops/s
AvajeBenchmark.avajeDeserializeStruct            thrpt   15   2593362.781 ±   44525.172  ops/s
AvajeBenchmark.avajeSerializeMediaContent        thrpt   15   1953477.274 ±   24879.426  ops/s
AvajeBenchmark.avajeSerializeStruct              thrpt   15   2433266.359 ±   44309.027  ops/s
AvajeBenchmark.furyDeserializeMediaContent       thrpt   15   8242244.006 ±   44929.164  ops/s
AvajeBenchmark.furyDeserializeStruct             thrpt   15  56411408.584 ±  377783.394  ops/s
AvajeBenchmark.furySerializeMediaContent         thrpt   15   9801305.268 ±  232625.435  ops/s
AvajeBenchmark.furySerializeStruct               thrpt   15  55036739.230 ± 1067573.666  ops/s
BenchmarkBase.furyDeserializeMediaContent        thrpt   15   8064191.098 ±  180262.953  ops/s
BenchmarkBase.furyDeserializeStruct              thrpt   15  56383272.199 ±  591648.257  ops/s
BenchmarkBase.furySerializeMediaContent          thrpt   15   9997628.016 ±  284824.951  ops/s
BenchmarkBase.furySerializeStruct                thrpt   15  57172381.091 ±  302062.876  ops/s
JacksonBenchmark.furyDeserializeMediaContent     thrpt   15   8407308.773 ±   47142.384  ops/s
JacksonBenchmark.furyDeserializeStruct           thrpt   15  57473268.779 ±  401459.521  ops/s
JacksonBenchmark.furySerializeMediaContent       thrpt   15  10193236.017 ±  278947.500  ops/s
JacksonBenchmark.furySerializeStruct             thrpt   15  57604328.310 ± 1269405.268  ops/s
JacksonBenchmark.jacksonDeserializeMediaContent  thrpt   15    694888.593 ±   21995.636  ops/s
JacksonBenchmark.jacksonDeserializeStruct        thrpt   15   1385459.015 ±   38102.697  ops/s
JacksonBenchmark.jacksonSerializeMediaContent    thrpt   15   1148172.170 ±   26327.076  ops/s
JacksonBenchmark.jacksonSerializeStruct          thrpt   15   1955240.876 ±   35842.791  ops/s
KryoBenchmark.furyDeserializeMediaContent        thrpt   15   8545050.614 ±   62051.741  ops/s
KryoBenchmark.furyDeserializeStruct              thrpt   15  54084943.025 ± 2309773.757  ops/s
KryoBenchmark.furySerializeMediaContent          thrpt   15  10681414.222 ± 1035724.879  ops/s
KryoBenchmark.furySerializeStruct                thrpt   15  53329127.448 ± 2907611.004  ops/s
KryoBenchmark.kryoDeserializeMediaContent        thrpt   15   1754468.356 ±   16250.788  ops/s
KryoBenchmark.kryoDeserializeStruct              thrpt   15   8014625.298 ±  264650.903  ops/s
KryoBenchmark.kryoSerializeMediaContent          thrpt   15   1399461.155 ±   39386.295  ops/s
KryoBenchmark.kryoSerializeStruct                thrpt   15   5034467.456 ±  134919.341  ops/s
```

The Microstream benchmarks all failed to run because that framework tries and fails to call an Unsafe method.

# Benchmark Setup (2023)
JMH config:
`-f 3 -wi 5 -i 5 -t 1 -w 2s -r 2s -rf csv`
OS: macOS Monterey
Hardware: 2.6 GHz 6-Core Intel Core i7
JDK: ???

# Benchmark Results
## Fury vs Jackson
Fury is 3x smaller for serialized binary size at most:
```java
furyMediaContentBytes size 336
furyStructBytes size 90
jacksonMediaContentBytes size 664
jacksonStructBytes size 278
```

The benchmark results here are for reference only. Jackson is a json format, fury is a binary format, which are suitable for different scenarios.
- Fury is 32.1x faster than jackson for Struct serialization
- Fury is 45x faster than jackson for Struct deserialization
- Fury is 8.8x faster than jackson for MediaContent serialization
- Fury is 11.8x faster than jackson for MediaContent deserialization

```java
Benchmark                                         Mode  Cnt         Score         Error  Units
JacksonBenchmark.furyDeserializeMediaContent     thrpt   15   2783918.470 ±   68075.351  ops/s
JacksonBenchmark.furyDeserializeStruct           thrpt   15  13686679.435 ± 1440816.885  ops/s
JacksonBenchmark.furySerializeMediaContent       thrpt   15   3410893.820 ±  265913.345  ops/s
JacksonBenchmark.furySerializeStruct             thrpt   15  17004519.876 ±  463107.804  ops/s
JacksonBenchmark.jacksonDeserializeMediaContent  thrpt   15    235938.122 ±   23054.241  ops/s
JacksonBenchmark.jacksonDeserializeStruct        thrpt   15    302600.964 ±   19586.158  ops/s
JacksonBenchmark.jacksonSerializeMediaContent    thrpt   15    386182.017 ±   38637.729  ops/s
JacksonBenchmark.jacksonSerializeStruct          thrpt   15    529044.347 ±   31945.826  ops/s
```
Compute speedup:
```python
import pandas as pd
df = pd.read_csv("jmh-result.csv")
s = df["Score"]
s[:4].to_numpy()/s[4:].to_numpy()
```
## Fury vs Microstream
Fury is 5x smaller for serialized binary size at most:
```java
furyMediaContentBytes size 336
furyStructBytes size 90
microstream mediaContentBytes size 1527
microstream structBytes size 120
```
Fury is :
```java
Benchmark                                                 Mode  Cnt         Score          Error  Units
MicrostreamBenchmark.furyDeserializeMediaContent         thrpt    3   2082751.515 ±  2638613.695  ops/s
MicrostreamBenchmark.furyDeserializeStruct               thrpt    3  10334292.992 ± 10542478.626  ops/s
MicrostreamBenchmark.furySerializeMediaContent           thrpt    3   2898943.216 ±   946978.784  ops/s
MicrostreamBenchmark.furySerializeStruct                 thrpt    3  13980885.316 ± 13424528.901  ops/s
MicrostreamBenchmark.microstreamDeserializeMediaContent  thrpt    3     76354.367 ±   231329.651  ops/s
MicrostreamBenchmark.microstreamSerializeMediaContent    thrpt    3     66316.198 ±   729572.587  ops/s
MicrostreamBenchmark.microstreamSerializeStruct          thrpt    3    233374.717 ±   732378.317  ops/s
```

## Fury vs Kryo in JDK17:
- Fury is 7.4x faster than kryo for MediaContent serialization
- Fury is 12.4x faster than kryo for Struct serialization
- Fury is 4.8x faster than kryo for Media deserialization
- Fury is 9.8x faster than kryo for Struct deserialization
```java
Benchmark                                   Mode  Cnt         Score         Error  Units
KryoBenchmark.furyDeserializeMediaContent  thrpt    9   2468765.648 ±  314266.100  ops/s
KryoBenchmark.furyDeserializeStruct        thrpt    9  20031536.933 ± 1893069.982  ops/s
KryoBenchmark.furySerializeMediaContent    thrpt    9   4041446.580 ±  402177.778  ops/s
KryoBenchmark.furySerializeStruct          thrpt    9  25475238.500 ±  920132.134  ops/s
KryoBenchmark.kryoDeserializeMediaContent  thrpt    9    545891.524 ±   19581.938  ops/s
KryoBenchmark.kryoDeserializeStruct        thrpt    9   2496417.446 ±   76754.451  ops/s
KryoBenchmark.kryoSerializeMediaContent    thrpt    9    478720.501 ±   69891.951  ops/s
KryoBenchmark.kryoSerializeStruct          thrpt    9   1942266.459 ±   63863.902  ops/s
```
<p align="center">
<img width="45%" alt="" src="images/bench_struct_tps.png">
<img width="45%" alt="" src="images/bench_media_content_tps.png">
</p>

## Fury vs Avaje
For serialized data size, fury is half of avaje:
```java
furyMediaContentBytes size 336
furyStructBytes size 70
avaje mediaContentBytes size 586
avaje structBytes size 149
```
- Fury is 5.6x faster than Avaje for Media serialization
- Fury is 29x faster than Avaje for Struct serialization
- Fury is 6x faster than Avaje for MediaContent deserialization
- Fury is 24x faster than Avaje for Struct deserialization
```java
Benchmark                                     Mode  Cnt         Score         Error  Units
AvajeBenchmark.avajeDeserializeMediaContent  thrpt    3    435874.584 ±   54036.329  ops/s
AvajeBenchmark.avajeDeserializeStruct        thrpt    3    906966.403 ± 1349025.028  ops/s
AvajeBenchmark.avajeSerializeMediaContent    thrpt    3    669107.596 ±   11450.125  ops/s
AvajeBenchmark.avajeSerializeStruct          thrpt    3    990601.474 ±   61731.364  ops/s
AvajeBenchmark.furyDeserializeMediaContent   thrpt    3   2607302.095 ± 2653734.646  ops/s
AvajeBenchmark.furyDeserializeStruct         thrpt    3  21689653.040 ± 2702955.367  ops/s
AvajeBenchmark.furySerializeMediaContent     thrpt    3   3775725.625 ±  273398.873  ops/s
AvajeBenchmark.furySerializeStruct           thrpt    3  28858705.061 ± 4330421.563  ops/s
```
<p align="center">
<img width="45%" alt="" src="images/avaje/bench_struct_tps.png">
<img width="45%" alt="" src="images/avaje/bench_media_content_tps.png">
</p>

## Fury vs Eclipse
For serialized data size, fury is 1/5 of eclipse:
```java
furyMediaContentBytes size 336
furyStructBytes size 70
eclipse mediaContentBytes size 1544
eclipse structBytes size 120
```
- Fury is 34x faster than eclipse for Media serialization
- Fury is 100x faster than eclipse for Struct serialization
- Fury is 30x faster than eclipse for MediaContent deserialization
- Fury is 52x faster than eclipse for Struct deserialization
```java
Benchmark                                         Mode  Cnt         Score         Error  Units
EclipseBenchmark.eclipseDeserializeMediaContent  thrpt    9    131575.419 ±    9426.819  ops/s
EclipseBenchmark.eclipseDeserializeStruct        thrpt    9    541499.957 ±  116080.027  ops/s
EclipseBenchmark.eclipseSerializeMediaContent    thrpt    9    160571.843 ±   13393.787  ops/s
EclipseBenchmark.eclipseSerializeStruct          thrpt    9    389589.337 ±   54602.631  ops/s
EclipseBenchmark.furyDeserializeMediaContent     thrpt    9   3929205.878 ±  100267.218  ops/s
EclipseBenchmark.furyDeserializeStruct           thrpt    9  28012278.714 ±  582665.499  ops/s
EclipseBenchmark.furySerializeMediaContent       thrpt    9   5401529.165 ±  325428.659  ops/s
EclipseBenchmark.furySerializeStruct             thrpt    9  39210806.902 ± 1638993.713  ops/s
```
<p align="center">
<img width="45%" alt="" src="images/eclipse/bench_struct_tps.png">
<img width="45%" alt="" src="images/eclipse/bench_media_content_tps.png">
</p>
