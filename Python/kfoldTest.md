
### kfold检验
[交叉检验(K-fold cross-validation)](http://www.cnblogs.com/Gavin_Liu/archive/2010/09/19/1830902.html)将原始地的数据等分成K份，1份做插值，剩下K-1份做检验。最常见的是K=10，也称为十折交叉检验。src/kfoldTest/kfoldTest.py里实现了对700多个站点的插值进行交叉检验。其难点在于对站点进行分组和获取检验点处的栅格值。

#### 1 随机分组
一种随机分成10组，每次取1份的实现方法。
```
#allsite为所有的站点信息
random.shuffle(allsite)
    n=len(allsite)
    nlist = [int(n*i*1.0/k) for i in range(1,k+1)]
    nlist.insert(0, 0)
    rmse_list=[] 
    for i in range(k):
        mlist=''; vlist=''
        for j in range(n):
            content=allsite[j]
            ai=string.atoi(allsite[j].split(' ')[0])
            if ((j<nlist[i])|(j>=nlist[i+1])):
                mlist=mlist + content
            else:
                vlist=vlist + content
        txt_path=r'D:\article\allsolar\txt'
        post_i=tspan+str(i)+int
        mod_path=write_txt(mlist, txt_path, 'm'+post_i)
        val_path=write_txt(vlist, txt_path, 'v'+post_i)
```
#### 2 获取检验值
若已知一个点，要获取该点上的栅格的属性值，可以用如下方法。
```
def extract_shp(point_shp, raster, i):
    res=[]
    outPointFeatures = r"D:\article\allsolar\extract\ep%s.shp" %i
    arcpy.CheckOutExtension("Spatial")
    ExtractValuesToPoints(point_shp, raster, outPointFeatures,"INTERPOLATE", "VALUE_ONLY")
    fields=['RASTERVALU']
    with arcpy.da.SearchCursor(outPointFeatures, fields) as cursor:
        for row in cursor:
            solar=u'{0}'.format(row[0])
            if solar==u'-9999.0':
                solar=u'0'            
            res.append(string.atof(solar))
    print 'extract: '+i
    return res
```
关键方法是ExtractValuesToPoints，详情请见[Arcpy帮助](http://help.arcgis.com/en/arcgisdesktop/10.0/help/index.html#//009z0000002t000000.htm).