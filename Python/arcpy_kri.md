### arcpy进行kriging插值

arcpy集成了很多地理处理工具，包括kriging插值，其语法和python相同，可以很方便地进行批量处理. src/arcpy_kri/mongoDB_arcpy_kri.py主要功能的是从mongoDB种读取数据，生成点shapefile文件，再用kriging方法对点shapefile文件进行插值。
#### 1 从mongoDB读取数据
```
client=MongoClient("localhost",27017)
db=client["meteorData"]
cursor=db.DataValuesM.find({"SiteID":string.atoi(id),"Date":tspan})
for content in cursor:
      #content是一个字典类型。
```
#### 2 生成点shapefile
```
gp = arcgisscripting.create()
#Set up inputs to tool
inTxt = r"%s\txt\%s.txt" %(root_path,shp_name)
inSep = "."
#Run tool
gp.CreateFeaturesFromTextFile(inTxt, inSep, outFile, r"D:\article\solar_data\shp\china54.shp")
```
关于CreateFeaturesFromTextFile请参考[Arcpy帮助](http://resources.arcgis.com/zh-cn/help/main/10.1/index.html#//00pv0000000z000000)
#### 3 kriging插值生成栅格
```
outKriging = Kriging(inFeatures, field, kModelOrdinary, cellSize)
raster_path=r'%s\raster0\%s.img' %(root_path,shp_name)
outKriging.save(raster_path)
```
关于Kriging请参考[Arcpy帮助](https://pro.arcgis.com/zh-cn/pro-app/tool-reference/3d-analyst/kriging.htm)