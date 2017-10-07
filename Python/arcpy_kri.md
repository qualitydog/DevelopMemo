### arcpy进行kriging插值

arcpy集成了很多地理处理工具，包括kriging插值，其语法和python相同，可以很方便地进行批量处理
用到的主要函数是：

···
outKriging = Kriging(inFeatures, field, kModelOrdinary, cellSize)
raster_path=r'%s\raster0\%s.img' %(root_path,shp_name)
outKriging.save(raster_path)
···



