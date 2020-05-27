# 学習メモ

## GIS実習オープン教材

- ベクトルデータとラスタデータが存在する
    - [ベクトルデータが位置を示す](https://gis-oer.github.io/gitbook/book/materials/00/00.html#%E3%83%99%E3%82%AF%E3%83%88%E3%83%AB%E3%83%87%E3%83%BC%E3%82%BF)
    - [ラスタデータはピクセルで区分されたデータ](https://gis-oer.github.io/gitbook/book/materials/00/00.html#%E3%83%A9%E3%82%B9%E3%82%BF%E3%83%87%E3%83%BC%E3%82%BF)


- 機能説明から
　- https://gis-oer.github.io/gitbook/book/materials/QGIS/QGIS.html#%E6%A9%9F%E8%83%BD%E8%AA%AC%E6%98%8E

### リモートセンシングとその解析

- https://gis-oer.github.io/gitbook/book/materials/06/06.html

- ランドブラウザのURLが違う　https://landbrowser.airc.aist.go.jp/landbrowser/


### QGIS with Python

- https://gis-oer.github.io/gitbook/book/materials/python/00/00.html

- コンソールから実行
    - プラグイン => Pythonコンソール

- QGIS ドキュメント　英語 https://docs.qgis.org/3.10/en/docs/ 
- QGIS ドキュメント　日本語　https://docs.qgis.org/3.10/ja/docs/index.html
- PyQGIS https://www.qgis.org/en/docs/index.html

- パネルの表示　ビュー　==>  パネル

- 経度緯度からの面積計算
    - 投影変換　実距離系への経度緯度からの変換
        - 投影変換解説　https://sites.google.com/site/qgisnoiriguchi/crs/02
            - パラメータを活用して、地図を平面な直角座標などに変換する
            - 座標系を実距離系へ変換する
            - 空間解析には必須
            - これ重要やなぁ。
    - 重心点
    　　- https://www.pasco.co.jp/recommend/cook/cook002/

    - Well Known Text https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry
        - Point、Line、Polygonなんかの記法のことのようだ

- QGISのプロンプトに打ち続けるのが面倒になってきたので空間辺りで終了。
- Pythonだけでできた方が良いなぁということから、そっちに       
    
## AUTO GIS

## 環境作成
https://automating-gis-processes.github.io/site/master/course-info/Installing_Anacondas_GIS.html

### インストールするパッケージ
#### 時間がある時に詳細を調査する

- geopandas
- geojson
- mapclassify
- contextily
- folium
- mplleaflet
- osmnx
- geoplot
- pysal 
- rasterio
- rasterstats
- pycrs

- [ツールへのリンク](https://automating-gis-processes.github.io/site/master/lessons/L1/Intro-Python-GIS.html#what-sort-of-tools-are-available-for-doing-gis-in-pure-python)


+++++++++
- pysalがうまくインストールできないが続ける

## Lesson1

Shapelyのクラスに関して

- Point
- LineString
- Polygon ---- MultiPoint
             + MultiLineString
             - MultiPoligon

## Lesson2

- データの読み書き　Geopandasを使う

- GeoPackages: are able to store both vector data and raster data
- CRS: Coordinate reference systems define how coordinates relate to real locations on the Earth. Geographic coordinate reference systems commonly use latitude and longitude degrees.
- Datum: defines the center point, orientation, and scale of the reference surface related to a coordinate reference system. Same coordinates can relate to different locations depending on the Datum! For example, WGS84 is a widely used global datum. ETRS89 is a datum used in Europe. Coordinate reference systems are often named based on the datum used.

- KML: Keyhole Markup Language / https://ja.wikipedia.org/wiki/KML

- 案外ファイルが見つからなくて困った
- 色々あるということ。shapefileは嫌われているのかも。

### Geopandas

- Pandas + Shapely + Fiona
- Pandasとの違いはgeometry列が含まれること
- geopandas は at を使って数字と文字の混ざったデータ取得が出来る
    - ex: df.at[0, "geometry"]
    - iterrows() / ふむ・良いなこれ
    - shapelyの関数などが使える　df.area() でgeometryのある部分の面積が求められる。なに！！！
    - to_file() でファイルを保存

### Map projections 

- CRS / 座標参照系　経度緯度は楕円体の地球をどう切り取るかみたいなところがあるので、その方法によって同じ経度・緯度でも異なる位置を示すことがある。その方法　https://tm23forest.com/contents/coordinate-reference-system
    - What's the best map projection?
    - CRSをそろえないと計算合わんぞ！！！
    - data.crs で座標参照系名が確認できる
    - to_crs() で座標参照系名が変更できる
    - pyprojのCRSクラスはたくさん情報を持っている

    - Natural Earth からデータとってきて遊ぼうぜ！！
        - https://www.naturalearthdata.com/
    - CRS.from_OO()　でCRSの種類を変更できる

### 距離を計測する

https://automating-gis-processes.github.io/site/master/notebooks/L2/calculating-distances.html

- GeoDataFrameとのデータフレームがあり、それにジオデータのデータフレームを作成できる。
- ふむ。確かにshpを読み込んだもののクラスもGeoDataFrameになっている
- Azimuthal Equidistant projection 正距方位図法　https://ja.wikipedia.org/wiki/%E6%AD%A3%E8%B7%9D%E6%96%B9%E4%BD%8D%E5%9B%B3%E6%B3%95
    - 簡単に言うと距離を正確に測るための図法のようである。でも方位に関しては適当っぽい。
    - ちなみにapplyはiterrows()よりめっちゃ早いぞって言っている
‐ Before exporting the data it is always good (basically necessary) to determine the coordinate reference system (projection) for the GeoDataFrame. 
    - どの系列で作られたデータか確認するのは大事だよ
‐ なければつけよう
    - We passed the coordinates as latitude and longitude decimal degrees, so the correct CRS is WGS84 (epsg code: 4326).

    new_data.crs = CRS.from_epsg(4326).to_wkt() # crsデータを渡す
    new_data.to_file("to_path.shp") #shpファイルの作成 

- .prjファイルを作るのに、crsの情報が必要のようだ
    - そういうのが入っているのね・・・
- pyprojパッケージのCRSクラスはrasterioをもとに作られているようだ
    - rasterioはMapboxがが作っている？　https://github.com/mapbox/rasterio/blob/master/docs/index.rst
    - rasterioはrasterデータにアクセスするためのパッケージ
    - mapbox凄いなぁ　https://github.com/mapbox

## Lesson3 
- geopandasのドキュメントを読むのも勉強になりそう（当たり前か
    - https://geopandas.org/data_structures.html

### Geocoding

- 住所を位置データに　Geopandasとgeopyを用いて
    - geopy : https://geopy.readthedocs.io/en/stable/
        - geopyは下のやつのもっといろんなソースを使えるモノっぽい。
    - geocoder: https://geocoder.readthedocs.io/ 
        - googleのAPIでシンプルに位置データ取れるっぽい

- geopandasのtools geocodeを使うと、経度緯度情報から住所を出してくれる！！
- 大量にやる場合、geopyのRateLimiterを活用しよう

### Table join 

- Pandas : https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html
    - いまいち各種の違いが分かっていない。軽く読むべきだな

- Table joins are really common procedures when doing GIS analyses. 
    - Pandasのmergeメソッドを使えよっていています。
    - インデックスでガチャっとやる場合は、JOINでできてしまいます。

    - 多分、MERGEはコラムの合致でやる場合だな。

### Point in Polygon & Intersect

#### How to check if point is inside a polygon?

- https://automating-gis-processes.github.io/site/master/notebooks/L3/point-in-polygon.html#How-to-check-if-point-is-inside-a-polygon?

    p1 = Point(24.952242, 60.1696017)
    p2 = Point(24.976567, 60.1612500)

    coords =[(24.950899, 60.169158), (24.953492, 60.169158), (24.953510, 60.170104), (24.950958, 60.169990)]
    poly = Polygon(coords)

    p1.within(poly) #/ True
    p2.within(poly) #/ False

### Intersect

- https://automating-gis-processes.github.io/site/master/notebooks/L3/point-in-polygon.html#Intersect

    line_a = LineString([(0,0), (1,1)])
    line_b = LineString([(1,1), (0,2)])
    line_a.intersect(line_b) #/ True
    line_a.touches(line_a) # / False

- ここは面白いが、データがどこにあるのかわからない。

- spatial index that can significantly boost the performance of your spatial queries.
    - R-tree https://en.wikipedia.org/wiki/R-tree

- spatialindex : Rtreeの構造を持つ
    - 空間インデックス：　https://desktop.arcgis.com/ja/arcmap/10.3/manage-data/geodatabases/
    an-overview-of-spatial-indexes-in-the-geodatabase.html
    - ええ感じに空間を分けてくれるっぽい
- https://automating-gis-processes.github.io/site/master/notebooks/L3/spatial_index.html
    - ここは重要。複数回確認する。

- 空間インデックスを作る
- それを活用して、候補を絞る
- その候補を用いて、抽出する
- これでかなりの時間を節約できる

- gpd.sjoin(): 空間で結合する
- df.merge(dff, on="colname"): データフレームを結合する
    - Spatial join / 次の授業課題だった 








