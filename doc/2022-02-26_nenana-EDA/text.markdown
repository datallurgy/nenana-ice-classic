```R
options(repr.plot.width=10, repr.plot.height=8)
```


```R
breakup <- breakup |>
    select(Year, Month, Day, Time, "Decimal Day")
```


    
    
    | Year|Month | Day|Time     | Decimal Day|
    |----:|:-----|---:|:--------|-----------:|
    | 2016|April |  23|15:39:00 |    114.6521|
    | 2017|May   |   1|12:00:00 |    121.5000|
    | 2018|May   |   1|13:18:00 |    121.5542|
    | 2019|April |  14|00:21:00 |    104.0146|
    | 2020|April |  27|12:56:00 |    118.5389|
    | 2021|April |  30|12:50:00 |    120.5347|



```R
breakup <- breakup |> 
    rename(Decimal_Day = "Decimal Day")
```


```R
mean_breakup_date <- mean(breakup$Decimal_Day)
```


```R
breakup_1980 <- breakup |>
    filter(Year >= 1980)

mean_breakup_date_1980 <- mean(breakup_1980$Decimal_Day)
```


```R
breakup$rollavg_9yr <- roll_mean(breakup$Decimal_Day, n = 9, align = "right", fill = NA)
```


```R
date_labels <- as.Date("1916-01-01")
yday(date_labels) <- seq(100, 150, 5)
date_labels <- date_labels |> strftime(format = "%b-%d")
```


```R
x <- as.Date("1917-01-01")
factor <- 1
```


```R
time_series_plot_1980 <- breakup |> ggplot() +
    geom_line(aes(x = Year, y = Decimal_Day)) +
    geom_line(aes(x = Year, y = rollavg_9yr),
              size = 1,
              color = '#2f873c') +
    geom_hline(yintercept = mean_breakup_date_1980, size = 1) +
    geom_vline(xintercept = 1980,
               size = 1,
               color = "red",
               linetype = "dashed") +
    scale_x_continuous(breaks = seq(1915, 2025, 10)) +
    scale_y_continuous(name = "Decimal Day of the Year",
                       breaks = seq(100, 150, 5),
                       sec.axis = sec_axis(~., breaks = seq(100, 150, 5),
                                           labels = date_labels)) +
    expand_limits(y = c(100, 150)) +
    labs(title = "Tenana River Breakup Dates") + 
    theme_linedraw() +
    theme(aspect.ratio=4/6,
          plot.title = element_text(hjust = 0.5))

time_series_plot_1980
```

    Warning message:
    “Removed 8 row(s) containing missing values (geom_path).”



    
![png](output_10_1.png)
    



```R
breakup_1980 <- breakup |>
    filter(Year >= 1980)
```


```R
data <- c(breakup_1980$Decimal_Day)
type <- rep('short', length(data))

short <- tibble(type, data)
```


```R
data <- c(breakup$Decimal_Day)
type <- rep('full', length(data))

full <- tibble(type, data)
```


```R
short_mean <- mean(short$data)
full_mean <- mean(full$data)
```


```R
complete <- rbind(short, full)
```


```R
density_plot <- complete |>
    ggplot(aes(x=data, fill=type, color=type)) +
    geom_density(alpha = 0.25) +
    scale_fill_manual(name = "Date Range", labels = c("1917-2021", '1980-2021'), values=c("#999999", "#2f873c")) +
    scale_color_manual(name = "Date Range", labels = c("1917-2021", '1980-2021'), values=c("#999999", "#2f873c")) +
    labs(title = "Density Plot of Breakup Dates", x="Day of the Year", y='Density') +
    theme_linedraw() +
    theme(aspect.ratio=4/6,
          plot.title = element_text(hjust = 0.5))

density_plot
```


    
![png](output_18_0.png)
    



```R
density_mean_plot <- complete |>
    ggplot(aes(x=data, fill=type, color=type)) +
    geom_density(alpha = 0.25) +
    scale_fill_manual(name = "Date Range", labels = c("1917-2021", '1980-2021'), values=c("#999999", "#2f873c")) +
    scale_color_manual(name = "Date Range", labels = c("1917-2021", '1980-2021'), values=c("#999999", "#2f873c")) +
    geom_vline(xintercept = short_mean,
               size = 1,
               color = "#2f873c",
               linetype = "dashed") +
    geom_vline(xintercept = full_mean,
               size = 1,
               color = "#999999",
               linetype = "dashed") +
    labs(title = "Density Plot of Breakup Dates", x="Day of the Year", y='Density') +
    theme_linedraw() +
    theme(aspect.ratio=4/6,
          plot.title = element_text(hjust = 0.5))

density_mean_plot
```


    
![png](output_19_0.png)
    



    
![png](output_20_0.png)
    


    `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    



    
![png](output_21_1.png)
    



```R

```
