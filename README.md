# Rendering Points Performance Test Sample for WinForms

### Description
This sample shows you how to render points in Map Suite Desktop (ForWinForms) Edition. 
By manipulating the follwoing two options, the points will be rendered differently. 
 - **Point Count**
 - **Grid Size in Pixel**
 
*Point Count*: controls how many points will be rendered.   
*Grid Size in Pixel*: is a way of optimization. For sample, there’s up to one point allowed to show in 3*3 square pixels.



Please refer to [Wiki](http://wiki.thinkgeo.com/wiki/map_suite_desktop_for_wpf) for the details.

![Screenshot](https://github.com/ThinkGeo/RenderingPointsPerformanceTestSample-ForWpf/blob/master/Screenshot.png)

### Requirements
This sample makes use of the following NuGet Package.

[MapSuiteDesktopForWinForms-BareBone 10.2.8](https://www.nuget.org/packages/MapSuiteDesktopForWinForms-BareBone/10.2.8/)

[ThinkGeo.MapSuite 11.0.0-beta060](https://www.nuget.org/packages/ThinkGeo.MapSuite/11.0.0-beta060/)

[ThinkGeo.MapSuite.Layers.Grids 11.0.0-beta013](https://www.nuget.org/packages/ThinkGeo.MapSuite.Layers.Grids/11.0.0-beta013/)

[ThinkGeo.MapSuite.ProductCenter 11.0.0-beta012](https://www.nuget.org/packages/ThinkGeo.MapSuite.ProductCenter/11.0.0-beta012/)

### About the Code

>**Adding points to the pointFeatureLayer**
```csharp
        for (int i = 0; i < pointCount; i++)
        {
            var randomX = random.NextDouble();
            var x = testExtent.UpperLeftPoint.X + testExtent.Width * randomX;
            var randomY = random.NextDouble();
            var y = testExtent.LowerLeftPoint.Y + testExtent.Height * randomY;
            
            pointFeatureLayer.InternalFeatures.Add(new Feature(x, y));
        }
            
        layerOverlay.Layers.Add(pointFeatureLayer);
```

>**Optimize the points in the pointFeatureLayer**
```csharp
        double resolutionX = boundingBox.Width / screenWidth * GridSizeInPixel;
        double resolutionY = boundingBox.Height / screenHeight * GridSizeInPixel;

        foreach (var feature in drawingFeatures)
        {
            byte[] wkb = feature.GetWellKnownBinary();
            double x = BitConverter.ToDouble(wkb, 5);
            double y = BitConverter.ToDouble(wkb, 13);

            int gridCol = Convert.ToInt32(Math.Floor((x - layerBoundingBox.UpperLeftPoint.X) / resolutionX));
            int gridRow = Convert.ToInt32(Math.Floor((layerBoundingBox.UpperLeftPoint.Y - y) / resolutionY));

            if (!grids.ContainsKey($"{gridCol}-{gridRow}"))
            {
                simplifiedDrawingFeautres.Add(feature);
                grids.Add($"{gridCol}-{gridRow}", true);
            }
        } 
```

### Getting Help

- [Map Suite Desktop for Wpf Wiki Resources](http://wiki.thinkgeo.com/wiki/map_suite_desktop_for_wpf)
- [Map Suite Desktop for Wpf Product Description](https://thinkgeo.com/ui-controls#desktop-platforms)
- [ThinkGeo Community Site](http://community.thinkgeo.com/)
- [ThinkGeo Web Site](http://www.thinkgeo.com)


### About Map Suite
Map Suite is a set of powerful development components and services for the .Net Framework.

### About ThinkGeo
ThinkGeo is a GIS (Geographic Information Systems) company founded in 2004 and located in Frisco, TX. Our clients are in more than 40 industries including agriculture, energy, transportation, government, engineering, software development, and defense.

