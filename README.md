# VSP-ConvertCRS

This code performs coordinate transformation on a CSV file containing well markers. Here is a brief overview of what the code does:

## Dependencies
- pandas
- pyproj

Make sure you have these libraries installed before running the code.

## Instructions
1. Import the required libraries:
   ```python
    import pandas as pd
    import pyproj
2. Define the path to the CSV file containing well markers:
   ```python
    path = "markers_input.csv"
3. Read the CSV file into a pandas DataFrame:
   ```python
    df = pd.read_csv(path, delimiter=";")
4. Cleanse the X and Y columns by replacing commas with periods and converting the values to float:
   ```python
    df['X'] = df['X'].str.replace(',', '.').astype(float)
    df['Y'] = df['Y'].str.replace(',', '.').astype(float)
5. Add 'longitude' and 'latitude' columns to the DataFrame with initial values:
   ```python
    df['longitude'] = 1.1111
    df['latitude'] = 1.1111
6. Define the EPSG codes for the source and target coordinate reference systems (CRS)
   ```python
    src_crs = pyproj.CRS("EPSG:3176")
    ll_crs = pyproj.CRS("EPSG:4326")
7. Define the transformation function transform that converts coordinates from the source CRS to the target CRS:
   ```python
    def transform(src_crs, ll_crs, point):
        # Create a transformer object to perform the transformation
        transformer = pyproj.Transformer.from_crs(src_crs, ll_crs)
        # Use the transformer to perform the transformation
        lon, lat = transformer.transform(*point)
        # Print the new coordinates (lat, long in EPSG 4326)
        print(f"New coordinates: ({lon:.6f}, {lat:.6f})")
        return round(lon, 7), round(lat, 7)
8. Iterate through each row of the DataFrame and apply the coordinate transformation using the transform function. Update the 'longitude' and 'latitude' columns with the transformed values:
   ```python
    for i in range(len(df)):
        lon, lat = transform(src_crs, ll_crs, (df.X[i], df.Y[i]))
        df['longitude'][i] = lon
        df['latitude'][i] = lat
9. Save the transformed DataFrame to a new CSV file:
   ```python
    df.to_csv('./markers_4326.csv')
    ```



