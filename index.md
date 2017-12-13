<center><img src="{{ site.baseurl }}/tutheaderbl.png" alt="Img"></center>

To add images, replace `tutheaderbl1.png` with the file name of any image you upload to your GitHub repository.

### Visualising time series in GGPLOT2

#### <a href="#section1"> 1. Introduction to the dataset</a>

#### <a href="#section2"> 2. Run rest of the code</a>

#### <a href="#section3"> 3. The third section</a>


The goal of this tutorial is to produce a line graph or time series plot with the mean daily temperature plus errors using GGPLOT2.Similarly produce a second graph of daily temperature fluctuations. Finally, plot and save the two figures together.
----------------------------------------


<a name="section1"></a>

## 1. Introduction to the dataset


At the beginning of your tutorial you can ask people to open `RStudio`, create a new script by clicking on `File/ New File/ R Script` set the working directory, and load some packages, for example `ggplot2` and `dplyr`. You can surround package names, functions, actions ("File/ New...") and small chunks of code with backticks, which defines them as inline code blocks and makes them stand out among the text, e.g. `ggplot2`.

When you have a larger chunk of code, you can paste the whole code in the `Markdown` document and add three backticks on the line before the code chunks starts and on the line after the code chunks ends. After the three backticks that go before your code chunk starts, you can specify in which language the code  is written, in our case `R`.

To find the backticks on your keyboard, look towards the top left corner on a Windows computer, perhaps just above `Tab` and before the number one key. On a Mac, look around the left `Shift` key. You can also just copy the backticks from below.

```r
# Set the working directory
setwd("your_filepath")

# Load packages
library(ggplot2)
library(dplyr)
library(gridExtra)
```

```r
# read the data
dat <- read.csv("temperatureTimeSeries.csv")

# look at the structure of the dataset
str(dat)

```

<a name="section2"></a>

## 2. Run rest of the code

You can add more text and code, e.g.

```r
# Create a plot of daily temperature fluctuations.
daily <- dat

# first we get the max and min values per day and per logger
daily2 <-   aggregate(data = daily,
              Temp~unique_ID+Date,
              FUN = function(x) c(max = max(x), min = min(x)))
daily2 <- do.call(data.frame, daily2)


# them we calculate the difference
daily2$diff <- daily2$Temp.max-daily2$Temp.min

daily3 <- aggregate(data = daily2,
                    diff~Date,
                    FUN = mean)


# housekeeping - formatting the date and time zone
daily3$Date2 <- as.POSIXct(daily3$Date, format="%Y-%m-%d", tz = "GMT")  
head(daily3)


# Make the plot
(temp.fluct <-  ggplot(data = daily3,
                   aes(x = Date2,
                       y = diff)) +
    geom_line(size = 1)+
    geom_smooth(method = "loess", span = 0.2, colour = "darkred", fill = "darkred") +
    xlab("Date") +
    ylab(expression(atop(paste("Daily Temperature Fluctuations"), ( degree~C)))) +
    theme_classic() +
    theme(text = element_text(size = 15)) +
    theme(legend.position = "none")
)



# Save figure
ggsave(filename = "Temperature daily fluctuations.pdf",
       plot = temp.fluct, height = 5, width = 5)

ggsave(filename = "temp_fluctuations.png",
       plot = temp.fluct, height = 5, width = 5)
```


Everything below this is footer material - text and links that appears at the end of all of your tutorials.

<hr>
<hr>

#### Check out our <a href="https://ourcodingclub.github.io/links/" target="_blank">Useful links</a> page where you can find loads of guides and cheatsheets.

#### If you have any questions about completing this tutorial, please contact us on ourcodingclub@gmail.com

#### <a href="INSERT_SURVEY_LINK" target="_blank">We would love to hear your feedback on the tutorial, whether you did it in the classroom or online!</a>

<ul class="social-icons">
	<li>
		<h3>
			<a href="https://twitter.com/our_codingclub" target="_blank">&nbsp;Follow our coding adventures on Twitter! <i class="fa fa-twitter"></i></a>
		</h3>
	</li>
</ul>

### &nbsp;&nbsp;Subscribe to our mailing list:
<div class="container">
	<div class="block">
        <!-- subscribe form start -->
		<div class="form-group">
			<form action="https://getsimpleform.com/messages?form_api_token=de1ba2f2f947822946fb6e835437ec78" method="post">
			<div class="form-group">
				<input type='text' class="form-control" name='Email' placeholder="Email" required/>
			</div>
			<div>
                        	<button class="btn btn-default" type='submit'>Subscribe</button>
                    	</div>
                	</form>
		</div>
	</div>
</div>
