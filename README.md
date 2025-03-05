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

- [Poster](https://github.com/RamonReyes632/DSU-Reswarch/blob/main/README.md#poster)


- [Data and Tables](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#data-and-tables-using-juliapython)
  
- [Power Point Presentation](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#power-point-presentation)
  
- [Gallery](https://github.com/RamonReyes632/DSU-Research/blob/main/README.md#gallery)



## Abstract
![IMG_3483](https://github.com/user-attachments/assets/6f02fd3f-ff20-4405-91be-0fb74f45181f)

## Poster (work in progress)
![Uploading Screenshot 2025-03-05 at 4.35.37 PM.png…]()



## Data and Tables (using Julia/Python)

### Bio-media Filtration

#### Dependencies
````julia
using CSV, CairoMakie , DataFrames, Dates, Statistics, Random, AlgebraOfGraphics
````
#### Data cleaning and visualization 

|   | Date       | Tank_Number | Filter_Treatment | Ammonia_mg_L | Nitrite_mg_L | Trial |
|---|------------|-------------|------------------|--------------|--------------|-------|
|1  | 0024-10-28 | 1           | CNTL             | 0.49         | 0.006        | 1     |
|2  | 0024-10-28 | 2           | Shavings         | 0.51         | 0.001        | 1     |
|3  | 0024-10-28 | 4           | Beads            | 0.49         | 0.002        | 1     |
|4  | 0024-10-28 | 5           | Beads            | 0.51         | 0.002        | 1     |
|5  | 0024-10-28 | 6           | Beads            | 0.52         | 0.002        | 1     |
|6  | 0024-10-28 | 7           | Shavings         | 0.49         | 0.001        | 1     |
|7  | 0024-10-28 | 8           | Shavings         | 0.52         | 0.001        | 1     |
|8  | 0024-10-28 | 10          | Bio balls        | 0.51         | 0.002        | 1     |
|9  | 0024-10-28 | 11          | Block            | 0.46         | 0.004        | 1     |
|10 | 0024-10-28 | 13          | CNTL             | 0.5          | 0.002        | 1     |
more...
|150| 0024-11-22 | 18          | Bio balls        | 0.01         | 0.002        | 2     |

````julia
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

#### Graphs
![output](https://github.com/user-attachments/assets/065d0ef1-d95d-45db-99f1-62bf83d55c1f)
````julia
with_theme() do
	### Time Series Plot for Ammonia Levels
	fig1 = Figure(Scene = (800, 400))
	ax1 = Axis(fig1[1,1], title="Ammonia Levels Over Time", xlabel="Date", ylabel="Ammonia (mg/L)")
	
	for treatment in treatments
	    sub_df = filter(row -> row.Filter_Treatment == treatment, df)
	    scatterlines!(ax1, sub_df.Date, sub_df.Ammonia_mg_L, label=treatment, color=color_dict[treatment])
	end
	axislegend(ax1)
	fig1
end
````

![output-2](https://github.com/user-attachments/assets/cef9a10d-9933-4fd3-aa6b-adb1c4876678)
````julia
with_theme() do
	### Time Series Plot for Nitrite Levels
	fig2 = Figure(Scene = (800, 400))
	ax2 = Axis(fig2[1,1], title="Nitrite Levels Over Time", xlabel="Date", ylabel="Nitrite (mg/L)")
	
	for treatment in treatments
	    sub_df = filter(row -> row.Filter_Treatment == treatment, df)
	    scatterlines!(ax2, sub_df.Date, sub_df.Nitrite_mg_L, label=treatment, color=color_dict[treatment])
	end
	axislegend(ax2)
	fig2
end
````

![ammoni vs](https://github.com/user-attachments/assets/7495b90e-a1c7-4f5a-b975-9119d6348224)
````julia
with_theme() do
    ### Scatter Plot: Ammonia vs. Nitrite (Colored by Filter Treatment)
    fig5 = Figure(Scene = (600, 500))
    ax5 = Axis(fig5[1,1], title="Ammonia vs. Nitrite Levels", xlabel="Ammonia (mg/L)", ylabel="Nitrite (mg/L)")

    # Get unique treatments and assign colors
    unique_treatments = unique(df.Filter_Treatment)
    color_palette = Makie.wong_colors(length(unique_treatments))
    color_dict = Dict(t => color_palette[i] for (i, t) in enumerate(unique_treatments))

    # Scatter plot with individual colors per treatment
    for treatment in unique_treatments
        sub_df = filter(row -> row.Filter_Treatment == treatment, df)
        scatter!(ax5, sub_df.Ammonia_mg_L, sub_df.Nitrite_mg_L, color=color_dict[treatment], markersize=8, alpha=0.7, label=treatment)
    end

    axislegend(ax5)  # Add legend to show filter treatments
    fig5
end
````

![output-3](https://github.com/user-attachments/assets/377542e1-b7e3-403f-8356-97ffc0f58c9f)
````julia
with_theme() do
    # 1. Box Plot: Ammonia Levels by Filtration Treatment
    fig3 = Figure()
    ax3 = Axis(fig3[1, 1], title="Ammonia Levels by Filtration Treatment", 
               xlabel="Filtration Treatment", ylabel="Ammonia (mg/L)")

    # Drop missing values
    df_filtered_ammonia = dropmissing(df, [:Filter_Treatment, :Ammonia_mg_L])

    # Ensure treatments are properly categorized
    unique_treatments_ammonia = unique(df_filtered_ammonia.Filter_Treatment)
    treatment_labels_ammonia = string.(unique_treatments_ammonia)
    treatment_to_num_ammonia = Dict(t => i for (i, t) in enumerate(unique_treatments_ammonia))

    # Map categorical treatments to numeric positions
    df_filtered_ammonia.Filter_Treatment_Num = [treatment_to_num_ammonia[t] for t in df_filtered_ammonia.Filter_Treatment]

    # Prepare data for boxplot
    positions_ammonia = df_filtered_ammonia.Filter_Treatment_Num
    values_ammonia = df_filtered_ammonia.Ammonia_mg_L

    if length(unique(positions_ammonia)) > 1
        boxplot!(ax3, positions_ammonia, values_ammonia, 
            color=:blue, 
            show_notch=false, 
            whiskerwidth=0.5, 
            mediancolor=:red, 
            outliercolor=:green, 
            width=0.6
        )
        ax3.xticks = (1:length(unique_treatments_ammonia), treatment_labels_ammonia)
    else
        error("Not enough valid data for a meaningful ammonia boxplot.")
    end

    fig3  # Display in Pluto.jl
end
````

![output-4](https://github.com/user-attachments/assets/245c986a-d079-44bf-ba3b-705c8c37d12b)
````julia
with_theme() do
    # 2. Box Plot: Nitrite Levels by Filtration Treatment
    fig4 = Figure()
    ax4 = Axis(fig4[1, 1], title="Nitrite Levels by Filtration Treatment", 
               xlabel="Filtration Treatment", ylabel="Nitrite (mg/L)")

    # Drop missing values
    df_filtered_nitrite = dropmissing(df, [:Filter_Treatment, :Nitrite_mg_L])

    # Ensure treatments are properly categorized
    unique_treatments_nitrite = unique(df_filtered_nitrite.Filter_Treatment)
    treatment_labels_nitrite = string.(unique_treatments_nitrite)
    treatment_to_num_nitrite = Dict(t => i for (i, t) in enumerate(unique_treatments_nitrite))

    # Map categorical treatments to numeric positions
    df_filtered_nitrite.Filter_Treatment_Num = [treatment_to_num_nitrite[t] for t in df_filtered_nitrite.Filter_Treatment]

    # Prepare data for boxplot
    positions_nitrite = df_filtered_nitrite.Filter_Treatment_Num
    values_nitrite = df_filtered_nitrite.Nitrite_mg_L

    if length(unique(positions_nitrite)) > 1
        boxplot!(ax4, positions_nitrite, values_nitrite, 
            color=:red, 
            show_notch=false, 
            whiskerwidth=0.5, 
            mediancolor=:black, 
            outliercolor=:green, 
            width=0.6
        )
        ax4.xticks = (1:length(unique_treatments_nitrite), treatment_labels_nitrite)
    else
        error("Not enough valid data for a meaningful nitrite boxplot.")
    end

    fig4  # Display in Pluto.jl
end
````

### Bio-media Filtration daily water quality

````julia
using CSV, CairoMakie , DataFrames, Dates, Statistics, Random
````

#### Data

|   | Date       | Tank_Number | Temperature_C | DO_mg_L | pH   | Filter_Treatment |
|---|------------|-------------|---------------|---------|------|------------------|
|1  | 0024-10-29 | 1           | 22.4          | 8.86    | 8.29 | CNTL             |
|2  | 0024-10-29 | 2           | 22.4          | 8.39    | 8.08 | Shavings         |
|3  | 0024-10-29 | 4           | 22.5          | 8.74    | 8.21 | Beads            |
|4  | 0024-10-29 | 5           | 22.5          | 8.83    | 8.26 | Beads            |
|5  | 0024-10-29 | 6           | 22.5          | 8.89    | 8.28 | Beads            |
|6  | 0024-10-29 | 7           | 22.6          | 8.65    | 8.14 | Shavings         |
|7  | 0024-10-29 | 8           | 22.5          | 8.54    | 8.07 | Shavings         |
|8  | 0024-10-29 | 10          | 22.5          | 8.78    | 8.22 | Bio balls        |
|9  | 0024-10-29 | 11          | 22.5          | 8.70    | 8.16 | Block            |
|10 | 0024-10-29 | 13          | 22.6          | 8.84    | 8.30 | CNTL             |
more...
|120| 0024-11-22 | 18          | 23.0          | 8.28    | 8.20 | Bio balls        |

````julia
begin
    # Load the CSV file
    file_path = "/Users/renzo/Documents/Ramons_Code/Files/Bio_media_measurements.csv"
    df = CSV.read(file_path, DataFrame, header=2)  # Skip the first row

    # Rename columns properly
    rename!(df, Dict(
        "Date" => :Date,
        "Tank Number" => :Tank_Number,
        "Temperature in Celcius" => :Temperature_C,
        "Do (mg/L)" => :DO_mg_L,
        "pH levels" => :pH,
		"Filter Treatment" => :Filter_Treatment
    ))

    # Convert data types
    df.Date = Dates.Date.(df.Date, "mm/dd/yy")  # Convert date column
    df.Tank_Number = Int.(df.Tank_Number)  # Ensure tank number is integer
    df.Temperature_C = Float64.(df.Temperature_C)  # Convert temperature to float
    df.DO_mg_L = Float64.(df.DO_mg_L)  # Convert DO to float
    df.pH = Float64.(df.pH)  # Convert pH to float
	df.Filter_Treatment  # Ensure tank number is integer

    # Save cleaned data
    CSV.write("Cleaned_Bio_media_measurements.csv", df)

    # Display cleaned data
    df#first(df, 5)
end
````

#### Graphs


````julia
with_theme() do
	# Compute moving averages
	window_size = 5
	df.Temperature_MA = [mean(df.Temperature_C[max(1, i-window_size+1):i]) for i in 1:nrow(df)]
	df.DO_MA = [mean(df.DO_mg_L[max(1, i-window_size+1):i]) for i in 1:nrow(df)]
	df.pH_MA = [mean(df.pH[max(1, i-window_size+1):i]) for i in 1:nrow(df)]
end
````

![rg1](https://github.com/user-attachments/assets/af40f6a7-49cc-490e-a699-a530391130ce)
````julia
with_theme() do
	# 1. Temperature over time with trend line
	fig1 = Figure()
	ax1 = Axis(fig1[1, 1], title="Temperature Over Time", xlabel="Date", ylabel="Temperature (°C)")
	scatter!(ax1, df.Date, df.Temperature_C, label="Raw Data")
	lines!(ax1, df.Date, df.Temperature_MA, color=:red, label="Moving Average")
	axislegend(ax1)
	#save("temperature_plot.png", fig1)
	fig1
end
````

![rg2](https://github.com/user-attachments/assets/85d25e75-ff83-4e6c-9342-3896453c6e66)
````julia
with_theme() do
	# 2. Dissolved Oxygen over time with trend line
	fig2 = Figure()
	ax2 = Axis(fig2[1, 1], title="Dissolved Oxygen Over Time", xlabel="Date", ylabel="DO (mg/L)")
	scatter!(ax2, df.Date, df.DO_mg_L, label="Raw Data")
	lines!(ax2, df.Date, df.DO_MA, color=:red, label="Moving Average")
	axislegend(ax2)
	#save("do_plot.png", fig2)
	fig2
end
````

![rg3](https://github.com/user-attachments/assets/1f54131a-77d2-4f79-961d-e1d8385bf39c)
````julia
with_theme() do
	fig3 = Figure()
	ax3 = Axis(fig3[1, 1], title="pH Levels Over Time", xlabel="Date", ylabel="pH")
	scatter!(ax3, df.Date, df.pH, label="Raw Data")
	lines!(ax3, df.Date, df.pH_MA, color=:red, label="Moving Average")
	axislegend(ax3)
	#save("ph_plot.png", fig3)
	fig3
end
````

![rg4](https://github.com/user-attachments/assets/c09d52ef-0aec-4ded-ad49-a50b0b8619a4)
````julia
with_theme() do
	fig4 = Figure()
	ax4 = Axis(fig4[1, 1], title="pH Level Distribution", xlabel="pH", ylabel="Frequency")
	hist!(ax4, df.pH, bins=10, color=:purple)
	#save("ph_distribution.png", fig4)
	fig4
end
````

![rg5](https://github.com/user-attachments/assets/b047d4be-113f-4ffc-8f9f-5e5637b61261)
````julia

with_theme() do
    # 5. Boxplot: DO Levels by Filter Treatment (Optimized)
    fig5 = Figure()
    ax5 = Axis(fig5[1, 1], title="DO Levels by Filter Treatment", xlabel="Filter Treatment", ylabel="Dissolved Oxygen (mg/L)")

    # Drop missing values to avoid errors
    df_filtered = dropmissing(df, [:Filter_Treatment, :DO_mg_L])

    # Ensure treatments are properly categorized
    unique_treatments = unique(df_filtered.Filter_Treatment)
    treatment_labels = string.(unique_treatments)  # Convert for x-axis labeling
    treatment_to_num = Dict(t => i for (i, t) in enumerate(unique_treatments))  # Assign unique index

    # Map categorical treatments to numeric positions
    df_filtered.Filter_Treatment_Num = [treatment_to_num[t] for t in df_filtered.Filter_Treatment]

    # Prepare data for boxplot
    positions = df_filtered.Filter_Treatment_Num
    values = df_filtered.DO_mg_L

    # Ensure there is enough data for a valid boxplot
    if length(unique(positions)) > 1  # Check if there are at least 2 categories
        boxplot!(ax5, positions, values, 
            color=:blue, 
            show_notch=false, 
            whiskerwidth=0.5, 
            mediancolor=:red, 
            outliercolor=:green, 
            width=0.6
        )
        ax5.xticks = (1:length(unique_treatments), treatment_labels)  # Ensure x-axis labels align
    else
        error("Not enough valid data for a meaningful boxplot.")
    end

    fig5  # Display in Pluto.jl
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






