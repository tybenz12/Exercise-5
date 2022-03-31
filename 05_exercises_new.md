---
title: 'Weekly Exercises #5'
author: "Ty Benz"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
library(ggnewscale)
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels and alt text.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
lettuce_harvest <- garden_harvest %>% filter(vegetable %in% c("lettuce"))

lettuce_graph <- lettuce_harvest %>% 
	count(variety) %>%
  mutate(variety = fct_reorder(variety, n, .desc = FALSE)) %>%
  ggplot(aes(x = n, y = variety)) + 
  geom_bar(stat = 'identity') +
  labs(x = "Number of Harvests", y = "Type of Lettuce") +
  ggtitle("Distribution of Harvests by Type of Lettuce") +
  theme(plot.title = element_text(hjust = 0.5))

ggplotly(lettuce_graph)
```

```{=html}
<div id="htmlwidget-35fd6af1fc4d3c6a1946" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-35fd6af1fc4d3c6a1946">{"x":{"data":[{"orientation":"v","width":[27,29,1,3,9],"base":[3.55,4.55,0.55,1.55,2.55],"x":[13.5,14.5,0.5,1.5,4.5],"y":[0.9,0.9,0.9,0.9,0.9],"text":["n: 27<br />variety: 0.9","n: 29<br />variety: 0.9","n:  1<br />variety: 0.9","n:  3<br />variety: 0.9","n:  9<br />variety: 0.9"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":148.310502283105},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Distribution of Harvests by Type of Lettuce","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0.5,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1.45,30.45],"tickmode":"array","ticktext":["0","10","20","30"],"tickvals":[0,10,20,30],"categoryorder":"array","categoryarray":["0","10","20","30"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Number of Harvests","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,5.6],"tickmode":"array","ticktext":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"tickvals":[1,2,3,4,5],"categoryorder":"array","categoryarray":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Type of Lettuce","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"6c7e12e135a1":{"x":{},"y":{},"type":"bar"}},"cur_data":"6c7e12e135a1","visdat":{"6c7e12e135a1":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```


```r
tomato_graph <- garden_harvest %>% 
  filter(vegetable %in% c("tomatoes")) %>% 
  group_by(variety, date) %>% 
  summarize(tot_weight_lbs = 0.00220462* sum(weight)) %>% 
  ggplot(aes(x = tot_weight_lbs, y = fct_reorder(variety, date, min, .desc = TRUE))) +
  geom_bar(stat = "identity") +
  ggtitle("Tomato Harvests Ordered by First Harvest Date") +
  labs(x = "Total Harvest Weight (lbs)", y = "Variety of Tomato") +
  theme(plot.title = element_text(hjust = 0.5))

ggplotly(tomato_graph)
```

```{=html}
<div id="htmlwidget-ca9f10ca41404cfbecd1" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-ca9f10ca41404cfbecd1">{"x":{"data":[{"orientation":"v","width":[1.02073906,0.46076558,0.43431014,0.21384814,1.12215158,0.84216484,0.3858085,1.11553772,1.41536604,1.1574255,1.2345872,1.29631656,2.19800614,2.2266662,0.943577360000001,3.39952404,3.086468,4.15791332,6.46835508,3.38850094,4.7619792,2.90348454,1.52559703999999,0.502653360000004,6.03624956,3.19008514,0.555564239999995,1.5763033,5.46304836,2.59042849999999,0.4850164,0.68784144,0.97444204,0.6393398,0.46517482,0.67902296,2.3038279,2.42949124,0.78043548,1.60275874,0.52469956,1.68432968,1.3558413,0.507062600000001,0.68122758,0.866415659999999,6.39119338,3.06883104,1.4770954,2.73152418,1.08908228,1.15963012,1.42418452,0.30203294,0.44753786,0.49163026,0.67681834,0.35714844,0.91050806,0.58642892,0.87523414,0.58201968,0.75838928,1.85629004,2.18918766,1.09569614,2.27737246,2.7888443,3.63541838,0.670204479999999,1.45284458,0.599656639999999,0.696659919999998,1.74385442,0.96121432,1.88935934,0.86641566,0.79145858,0.64154442,0.69886454,1.27647498,3.39070556,3.46786726,0.476197920000001,0.520290319999999,0.8267325,0.74736618,0.3086468,0.32628376,0.33730686,0.9590097,0.34392072,0.85318794,1.24120106,0.79145858,1.24340568,0.39462698,0.67681834,0.73193384,1.56748482,1.19710866,0.80248168,0.59745202,1.39331984,0.727524600000001,0.593042779999999,1.38670598,0.341716100000003,1.5652802,2.31926024,1.57409868,1.05160374,0.850983320000001,0.7054784,0.50926722,0.6393398,0.78484472,0.48060716,0.6724091,0.7385477,1.06483146,0.98326052,1.92022402,2.29721404,1.0141252,0.392422359999999,0.9479866,1.57409868,0.921531159999999,0.54454114,0.5291088,0.67681834,1.36465978,0.66799986,0.67902296,0.47619792,1.76810524,1.92242864,3.52959662,1.30733966,1.32056738,0.44312862,0.48281178,0.05291088,0.18959732,0.06834322,0.23368972,0.28880522,0.0881848,0.20062042,0.220462,0.37037616,0.22487124,0.26014516,0.47840254,0.64374904,0.17857422,0.14109568,0.66579524,0.928145020000001,0.277782119999999,1.05160374,0.96121432,0.29982832,0.994283619999999,1.08687766,0.584224300000001,0.96121432,0.1653465,1.11553772,1.80558378,0.837755599999999,1.83644846,2.49342522,0.97444204,1.3558413,1.12215158,0.568791959999999,2.33248796,1.80558378,1.49473236,3.03576174,0.6944553,1.23899644,0.40565008,1.30293042,0.53131342,0.39903622,0.7936632,1.27427036,1.65787424,1.70417126,2.59704236,1.66228348,0.76279852,1.76590062,0.74736618,1.72180822,0.80248168,1.34040896,1.55205248,2.26194012,2.41846814,1.29852118,2.64995324,0.562178099999997,0.696659920000002,4.42246772,0.720910740000001,0.469584059999999,0.55997348,0.440923999999999,1.89376858,1.34702282,1.39552446,0.74075232,0.51367646,0.52469956,1.32056738,0.2314851,0.535722659999999,1.76590062,0.253531300000001,1.46827692,1.89817782,1.7747191,1.48591388,3.59573522,4.01902226,0.800277059999999,1.97974876,1.06703608,0.16093726,0.14770954,0.11904948,0.3527392,1.0802638,0.72311536,0.67461372,0.32628376,0.67461372,0.73413846,1.23899644,1.07585456,0.575405819999999,2.87261986,1.81219764,4.30562286,1.3448182,2.72050108,5.24038174,0.910508060000002,1.5983495,0.467379439999998,6.46835508,0.209438900000002,4.36073836,0.154323400000003,0.141095679999999,4.35853374,0.507062599999998,6.25671156],"base":[6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,6.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,7.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,9.55,1.55,1.55,1.55,1.55,1.55,1.55,1.55,1.55,1.55,1.55,1.55,1.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,10.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,2.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,8.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,11.55,3.55,3.55,3.55,3.55,3.55,3.55,3.55,3.55,3.55,3.55,3.55,3.55,3.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,5.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,4.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55,0.55],"x":[0.51036953,1.25112185,1.69865971,2.02273885,2.69073871,3.67289692,4.28688359,5.0375567,6.30300858,7.58940435,8.7854107,10.05086258,11.79802393,14.0103601,15.59548188,17.76703258,21.0100286,24.63221926,29.94535346,34.87378147,38.94902154,42.78175341,44.9962942,46.0104194,49.27987086,53.89303821,55.7658629,56.83179667,60.3514725,64.37821093,0.2425082,0.82893712,1.66007886,2.46696978,3.01922709,3.59132598,5.08275141,7.44941098,9.05437434,10.24597145,11.3097006,12.41421522,13.93430071,14.86575266,15.45989775,16.23371937,19.86252389,24.5925361,26.86549932,28.96980911,30.88011234,32.00446854,33.29637586,0.15101647,0.52580187,0.99538593,1.57961023,2.09659362,2.73042187,3.47889036,4.20972189,4.9383488,5.60855328,6.91589294,8.93863179,10.58107369,12.26760799,14.80071637,18.01284771,20.16565914,21.22718367,22.25343428,22.90159256,24.12184973,0.48060716,1.90589399,3.28378149,4.11271861,4.82922011,5.49942459,6.48709435,8.82068462,12.24997103,14.22200362,14.72024774,15.39375915,0.37368309,0.90168958,1.21915486,1.55095017,2.19910845,2.85057366,3.44912799,4.49632249,5.51265231,6.53008444,7.34910077,7.88482343,8.58919952,9.73890885,11.12120559,12.12100076,12.82096761,13.81635354,14.87677576,15.53705945,16.52693383,17.39114487,18.34464302,20.28691324,22.2335927,23.54644391,24.49773744,0.3527392,0.96011201,1.53441552,2.24650778,2.87923372,3.45574185,4.16122025,5.06290983,6.08695582,7.53869809,9.64741712,11.30308674,12.00636052,12.676565,13.93760764,15.18542256,0.27227057,0.80909554,1.41205911,2.43279817,3.44912799,4.1226394,4.70024984,5.82240142,7.66766836,10.39368099,12.81214913,14.12610265,15.00795065,15.47092085,0.02645544,0.14770954,0.27667981,0.42769628,0.68894375,0.87743876,1.02184137,1.23238258,1.52780166,1.82542536,2.06793356,2.43720741,2.9982832,3.40944483,3.56927978,3.97272524,4.76969537,5.37265894,6.03735187,7.0437609,7.67428222,8.32133819,9.36191883,10.19746981,10.97018912,11.53346953,12.17391164,13.63447239,14.95614208,16.29324411,18.45818095,20.19211458,21.35725625,22.59625269,23.44172446,24.89236442,26.96140029,28.61155836,30.87680541,0.34722765,1.31395352,2.13627678,2.99056703,3.90768895,4.37286377,4.96921348,6.00318026,7.46925256,9.15027531,11.30088212,13.43054504,14.64308604,0.88295031,2.13958371,3.37417091,4.63631586,5.70776118,7.1539919,9.0609882,11.40119233,13.25968699,15.2339242,16.83998987,17.46940888,20.0289727,22.60066193,23.19590933,23.7106881,24.21113684,25.37848313,0.67351141,2.04478505,3.11292344,3.74013783,4.25932584,5.18195931,5.95798555,6.34158943,7.49240107,8.50211703,9.36302114,11.04624851,12.88269697,14.51301346,17.05383801,20.86121675,23.27086641,24.66087932,26.18427174,0.08046863,0.23479203,0.36817154,0.60406588,1.32056738,2.22225696,2.9211215,3.42157024,3.92201898,4.62639507,5.61296252,6.77038802,7.59601821,9.32003105,11.6624398,14.72135005,17.54657058,19.57923022,23.55967163,26.63511653,27.88954531,28.92240978,32.39027704,35.72917403,38.01426266,40.27179354,40.41950308,42.66931779,45.10211596,48.48400304],"y":[0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.9],"text":["tot_weight_lbs: 1.02073906<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.46076558<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.43431014<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.21384814<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.12215158<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.84216484<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.38580850<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.11553772<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.41536604<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.15742550<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.23458720<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.29631656<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.19800614<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.22666620<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.94357736<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.39952404<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.08646800<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 4.15791332<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 6.46835508<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.38850094<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 4.76197920<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.90348454<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.52559704<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.50265336<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 6.03624956<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.19008514<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.55556424<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.57630330<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 5.46304836<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.59042850<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.48501640<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.68784144<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.97444204<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.63933980<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.46517482<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67902296<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.30382790<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.42949124<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.78043548<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.60275874<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.52469956<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.68432968<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.35584130<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.50706260<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.68122758<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.86641566<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 6.39119338<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.06883104<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.47709540<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.73152418<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.08908228<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.15963012<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.42418452<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.30203294<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.44753786<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.49163026<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67681834<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.35714844<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.91050806<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.58642892<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.87523414<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.58201968<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.75838928<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.85629004<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.18918766<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.09569614<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.27737246<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.78884430<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.63541838<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67020448<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.45284458<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.59965664<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.69665992<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.74385442<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.96121432<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.88935934<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.86641566<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.79145858<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.64154442<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.69886454<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.27647498<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.39070556<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.46786726<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.47619792<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.52029032<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.82673250<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.74736618<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.30864680<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.32628376<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.33730686<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.95900970<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.34392072<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.85318794<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.24120106<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.79145858<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.24340568<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.39462698<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67681834<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.73193384<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.56748482<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.19710866<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.80248168<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.59745202<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.39331984<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.72752460<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.59304278<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.38670598<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.34171610<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.56528020<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.31926024<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.57409868<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.05160374<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.85098332<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.70547840<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.50926722<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.63933980<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.78484472<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.48060716<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67240910<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.73854770<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.06483146<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.98326052<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.92022402<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.29721404<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.01412520<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.39242236<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.94798660<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.57409868<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.92153116<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.54454114<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.52910880<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67681834<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.36465978<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.66799986<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67902296<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.47619792<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.76810524<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.92242864<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.52959662<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.30733966<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.32056738<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.44312862<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.48281178<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.05291088<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.18959732<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.06834322<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.23368972<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.28880522<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.08818480<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.20062042<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.22046200<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.37037616<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.22487124<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.26014516<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.47840254<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.64374904<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.17857422<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.14109568<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.66579524<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.92814502<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.27778212<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.05160374<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.96121432<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.29982832<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.99428362<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.08687766<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.58422430<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.96121432<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.16534650<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.11553772<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.80558378<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.83775560<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.83644846<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.49342522<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.97444204<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.35584130<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.12215158<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.56879196<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.33248796<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.80558378<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.49473236<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.03576174<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.69445530<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.23899644<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.40565008<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.30293042<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.53131342<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.39903622<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.79366320<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.27427036<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.65787424<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.70417126<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.59704236<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.66228348<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.76279852<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.76590062<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.74736618<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.72180822<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.80248168<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.34040896<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.55205248<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.26194012<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.41846814<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.29852118<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.64995324<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.56217810<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.69665992<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 4.42246772<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.72091074<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.46958406<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.55997348<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.44092400<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.89376858<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.34702282<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.39552446<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.74075232<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.51367646<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.52469956<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.32056738<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.23148510<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.53572266<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.76590062<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.25353130<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.46827692<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.89817782<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.77471910<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.48591388<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 3.59573522<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 4.01902226<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.80027706<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.97974876<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.06703608<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.16093726<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.14770954<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.11904948<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.35273920<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.08026380<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.72311536<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67461372<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.32628376<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.67461372<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.73413846<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.23899644<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.07585456<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.57540582<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.87261986<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.81219764<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 4.30562286<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.34481820<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 2.72050108<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 5.24038174<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.91050806<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 1.59834950<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.46737944<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 6.46835508<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.20943890<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 4.36073836<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.15432340<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.14109568<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 4.35853374<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 0.50706260<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9","tot_weight_lbs: 6.25671156<br />fct_reorder(variety, date, min, .desc = TRUE): 0.9"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":113.24200913242},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Tomato Harvests Ordered by First Harvest Date","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0.5,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-3.283671259,68.957096439],"tickmode":"array","ticktext":["0","20","40","60"],"tickvals":[0,20,40,60],"categoryorder":"array","categoryarray":["0","20","40","60"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Total Harvest Weight (lbs)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,12.6],"tickmode":"array","ticktext":["volunteers","Black Krim","Brandywine","Jet Star","Old German","Mortgage Lifter","Amish Paste","Better Boy","Cherokee Purple","Big Beef","Bonny Best","grape"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12],"categoryorder":"array","categoryarray":["volunteers","Black Krim","Brandywine","Jet Star","Old German","Mortgage Lifter","Amish Paste","Better Boy","Cherokee Purple","Big Beef","Bonny Best","grape"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Variety of Tomato","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"6c7ec0543a7":{"x":{},"y":{},"type":"bar"}},"cur_data":"6c7ec0543a7","visdat":{"6c7ec0543a7":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
  
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
train_trips <- small_trains %>% 
  group_by(departure_station, month, year) %>% 
  mutate(date = zoo::as.yearmon(paste(year, month), "%Y %m"), 
         trip = paste(departure_station, arrival_station, sep = "-")) %>% 
  ungroup()
  
lyon_trips <- train_trips %>% 
  filter(departure_station %in% c("PARIS LYON")) %>% 
  ggplot(aes(x = date, y = total_num_trips)) +
  geom_line() +
  labs(title = "Number of Trips from Paris Lyon Station", 
       subtitle = "Trip: {closest_state}",
       x = "",
       y = "") +
  theme(legend.position = "none") +
  transition_states(trip, 
                    transition_length = 2, 
                    state_length = 4)

animate(lyon_trips, nframes = 200, duration = 30)
anim_save("lyon_trips.gif")
```

![](lyon_trips.gif)<!-- -->


## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. I have filtered the data to the tomatoes and find the *daily* harvest in pounds for each variety. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0. 
  You should do the following:
  * For each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each variety and arranged (HINT: `fct_reorder()`) from most to least harvested weights (most on the bottom).  
  * Add animation to reveal the plot over date. Instead of having a legend, place the variety names directly on the graph (refer back to the tutorial for how to do this).


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, 
           date, 
           fill = list(daily_harvest_lb = 0)) %>% 
  group_by(variety) %>% 
  mutate(cum_wt_lbs = cumsum(daily_harvest_lb))  %>% 
  ggplot(aes(x = date, y = cum_wt_lbs, fill = fct_reorder(variety, cum_wt_lbs, .fun = max, .desc = TRUE))) +
  geom_area() +
  geom_text(aes(label = variety), position = "stack") +
  labs(title = "Tomato Harvest Over Time", 
       subtitle = "Date: {frame_along}",
       x = "",
       y = "",
       color = "vegetable",
       shape = "vegetable") +
  theme(legend.position = "none", 
        axis.title.x = element_blank(), 
        axis.title.y = element_blank()) +
  transition_reveal(date)
  
anim_save("tomato_area.gif")
```
![](tomato_area.gif)<!-- -->


## Maps, animation, and movement!

  4. Map Lisa's `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
mallorca_map <- get_stamenmap(
    bbox = c(left = 2.3263, bottom = 39.49, right = 2.6181, top = 39.7), 
    maptype = "terrain",
    zoom = 12)

ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7, 
             aes(x = lon, y = lat, color = ele),
             size = .5) + 
  geom_point(data = mallorca_bike_day7, 
             aes(x = lon, y = lat),
             size = .5, color = "red") +
  scale_color_viridis_c(option = "magma") +
  theme_map() +
  theme(legend.background = element_blank()) +
  labs(title = "Mallorca Bike Ride Day 7",
       subtitle = "Time: {frame_along}",
       x = "",
       y = "",
       color = "Elevation") +
  transition_reveal(time)

anim_save("mallorca_bike.gif")
```
  
![](mallorca_bike.gif)<!-- -->
  
  
  5. In this exercise, you get to meet Lisa's sister, Heather! She is a proud Mac grad, currently works as a Data Scientist where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files putting them in swim, bike, run order (HINT: `bind_rows()`), 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_triathlon <- bind_rows(panama_swim, panama_bike, panama_run)

panama_map <- get_stamenmap(
    bbox = c(left = -79.5834, bottom = 8.8905, right = -79.4375, top = 9.0235), 
    maptype = "terrain",
    zoom = 13)

panama_anim <- ggmap(panama_map) +
  geom_path(data = panama_triathlon, 
             aes(x = lon, y = lat, color = ele),
             size = .5) + 
  scale_color_viridis_c(option = "magma") +
  labs(color = "Elevation") +
  new_scale_color() +
  geom_point(data = panama_triathlon, 
             aes(x = lon, y = lat, color = event),
             size = .5) +
  scale_color_manual(values = c("green", "red", "blue")) +
  theme_map() +
  theme(legend.background = element_blank()) +
  labs(title = "Panama Triathlon",
       subtitle = "Time: {frame_along}",
       x = "",
       y = "",
       color = "Event") +
  transition_reveal(time)
  
animate(panama_anim, nframes = 200, duration = 30)
anim_save("panama_triathlon.gif")
```
 
![](panama_triathlon.gif)<!-- -->
  
## COVID-19 data

  6. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for the the 15th of each month. So, filter only to those dates - there are some lubridate functions that can help you do this.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")

new_covid_data <- covid19 %>% 
  mutate(state = str_to_lower(state), day = day(date)) %>% 
  left_join(census_pop_est_2018,
            by = c("state")) %>% 
  mutate(covid_per_10000 = (cases/est_pop_2018)*10000) %>% 
  filter(day %in% c("15"))
```

```r
covid_anim <- new_covid_data %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = covid_per_10000, 
               group = date)) +
  scale_fill_viridis_c(option = "magma") +
  expand_limits(x = states_map$long, y = states_map$lat) + 
  theme_map() +
  labs(title = "COVID Cases per 10,000 by State", 
       subtitle = "Date: {frame_time}", 
       fill = "Cases per 10,000") +
  transition_time(date)

animate(covid_anim, nframes = 200, end_pause = 10)
anim_save("covid_map.gif")
```

![](covid_map.gif)<!-- -->

## Your first `shiny` app (for next week!)

  7. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. You should create a new project for the app, separate from the homework project. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' daily number of COVID cases per 100,000 over time. The x-axis will be date. You will have an input box where the user can choose which states to compare (`selectInput()`), a slider where the user can choose the date range, and a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
Put the link to your app [here](https://ty-benz.shinyapps.io/ShinyApp/)
  
## GitHub link

  8. Below, provide a link to your GitHub repo with this set of Weekly Exercises. 


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
