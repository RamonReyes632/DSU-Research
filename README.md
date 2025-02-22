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

### Bio-media Filtration daily water quality

#### Dependencies
````julia
using CSV, CairoMakie , DataFrames, Dates, Statistics, Random, AlgebraOfGraphics
````
#### Data cleaning and visualization 
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






