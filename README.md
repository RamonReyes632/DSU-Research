## DSU-Research

# Bio Filter Media Project


## References/Footnotes

Ramon Reyes Rivas[^1]
Grant S. Blank[^2]
Angel Calon-Zapata[^3]
Dennis McIntosh[^4]

[^1]: Ramon Reyes Rivas.
[^2]: Grant S. Blank.
[^3]: Angel Calon-Zapata.
[^4]​: Dennis McIntosh.  

## Table of contents

- [Abstract](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#abstract)

- [Poster](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#poster-work-in-progress)

- [Data and Tables](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#data-and-tables-using-juliapython)
  
- [Power Point Presentation](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#power-point-presentation)
  
- [Gallery](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#gallery)



## Abstract
![IMG_3483](https://github.com/user-attachments/assets/6f02fd3f-ff20-4405-91be-0fb74f45181f)

## Poster (work in progress)
![Slide1](https://github.com/user-attachments/assets/707495b9-9ce4-4efb-b6b7-c407d24186f1)


## Data and Tables (using Julia/Python)
Example
```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```

````julia
using CSV, CairoMakie , DataFrames, Dates, Statistics, Random, AlgebraOfGraphics


Data 

begin
    # Load the CSV file
    file_path = "/Users/renzo/Documents/Ramons_Code/Files/Bio_media_sheet_1.csv"
    df = CSV.read(file_path, DataFrame, header=2)  # Skip the first row

    # Rename columns properly
    rename!(df, Dict(
        "Date" => :Date,
        "Tank Number" => :Tank_Number,
        "Filtration Treatment " => :Filter_Treatment,  # Trimmed space in column name
        "Ammonia" => :Ammonia_mg_L,
        "Nitrite" => :Nitrite_mg_L,
        "Trial" => :Trial
    ))

    # Convert data types
    df.Date = Date.(df.Date, dateformat"mm/dd/yy")  # Corrected date format
    df.Tank_Number = Int.(df.Tank_Number)  # Ensure tank number is integer
    df.Ammonia_mg_L = Float64.(df.Ammonia_mg_L)  # Convert Ammonia to float
    df.Nitrite_mg_L = Float64.(df.Nitrite_mg_L)  # Convert Nitrite to float
	
    # Save cleaned data
    #CSV.write("Cleaned_Bio_media_measurements_corrected.csv", df)

    # Display cleaned data
    df
end
````


## Power Point Presentation

![Slide1](https://github.com/user-attachments/assets/51adfdf0-8bce-4e0e-890b-8938a8949921)

![Slide2](https://github.com/user-attachments/assets/d784e939-49ea-4477-8ca6-d397ecf62745)

![Slide3](https://github.com/user-attachments/assets/ba19a121-fed7-4067-a63a-31cf9c6a8997)

![Slide4](https://github.com/user-attachments/assets/03fc9746-dd15-47c6-9bda-3d7010129b75)

![Slide5](https://github.com/user-attachments/assets/aa7bd93b-5b4c-4e84-9751-c9812b2c3536)

![Slide6](https://github.com/user-attachments/assets/43746750-4d9d-47a5-bda4-0cbcbda54e9b)

![Slide7](https://github.com/user-attachments/assets/246e4874-5a52-4be5-bae4-df5f1e2c3dae)

![Slide8](https://github.com/user-attachments/assets/93cb313c-eafe-4d64-993f-6df94129ab15)

![Slide9](https://github.com/user-attachments/assets/0ddc33c3-a942-4e4f-b209-60f24877b0c5)


## Gallery
![IMG_0715](https://github.com/user-attachments/assets/0bbc22c7-013e-4787-925f-0a253e9b187d)

![IMG_3486](https://github.com/user-attachments/assets/aad2edc2-4421-4774-b6a8-e789de7a81d7)






