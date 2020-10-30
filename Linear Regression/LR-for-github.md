Linear Regression
================
Ayushi
27/10/2020

### Linear Regression:

Linear Regression is a supervised machine learning algorithm. It
establish relationship between continuous independent and continuous
dependent variable

![](E:/LR.png)

Image
source:[link](https://www.javatpoint.com/linear-regression-in-machine-learning)

Linear regression performs the task to predict a dependent variable
value (y) based on a given independent variable (x). So, this regression
technique finds out a linear relationship between x (input) and
y(output). Hence, the name is Linear Regression.

#### \>**Linear regression implementation using R**

##### **Case Study:** *House Price Prediction*

> Import dataset

``` r
house<-read.csv("E:/Imarticus/R/house.csv")
head(house) 
```

    ##           id            date   price bedrooms bathrooms sqft_living sqft_lot
    ## 1 7129300520 20141013T000000  221900        3      1.00        1180     5650
    ## 2 6414100192 20141209T000000  538000        3      2.25        2570     7242
    ## 3 5631500400 20150225T000000  180000        2      1.00         770    10000
    ## 4 2487200875 20141209T000000  604000        4      3.00        1960     5000
    ## 5 1954400510 20150218T000000  510000        3      2.00        1680     8080
    ## 6 7237550310 20140512T000000 1230000        4      4.50        5420   101930
    ##   floors waterfront view condition grade sqft_above sqft_basement yr_built
    ## 1      1          0    0         3     7       1180             0     1955
    ## 2      2          0    0         3     7       2170           400     1951
    ## 3      1          0    0         3     6        770             0     1933
    ## 4      1          0    0         5     7       1050           910     1965
    ## 5      1          0    0         3     8       1680             0     1987
    ## 6      1          0    0         3    11       3890          1530     2001
    ##   yr_renovated zipcode     lat     long sqft_living15 sqft_lot15
    ## 1            0   98178 47.5112 -122.257          1340       5650
    ## 2         1991   98125 47.7210 -122.319          1690       7639
    ## 3            0   98028 47.7379 -122.233          2720       8062
    ## 4            0   98136 47.5208 -122.393          1360       5000
    ## 5            0   98074 47.6168 -122.045          1800       7503
    ## 6            0   98053 47.6561 -122.005          4760     101930

> Check structure of data

``` r
str(house)
```

    ## 'data.frame':    21613 obs. of  21 variables:
    ##  $ id           : num  7.13e+09 6.41e+09 5.63e+09 2.49e+09 1.95e+09 ...
    ##  $ date         : Factor w/ 372 levels "20140502T000000",..: 165 221 291 221 284 11 57 252 340 306 ...
    ##  $ price        : num  221900 538000 180000 604000 510000 ...
    ##  $ bedrooms     : int  3 3 2 4 3 4 3 3 3 3 ...
    ##  $ bathrooms    : num  1 2.25 1 3 2 4.5 2.25 1.5 1 2.5 ...
    ##  $ sqft_living  : int  1180 2570 770 1960 1680 5420 1715 1060 1780 1890 ...
    ##  $ sqft_lot     : int  5650 7242 10000 5000 8080 101930 NA 9711 7470 6560 ...
    ##  $ floors       : num  1 2 1 1 1 1 2 1 1 2 ...
    ##  $ waterfront   : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ view         : int  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ condition    : int  3 3 3 5 3 3 3 3 3 3 ...
    ##  $ grade        : int  7 7 6 7 8 11 7 7 7 7 ...
    ##  $ sqft_above   : int  1180 2170 770 1050 1680 3890 1715 1060 1050 1890 ...
    ##  $ sqft_basement: int  0 400 0 910 0 1530 0 0 730 0 ...
    ##  $ yr_built     : int  1955 1951 1933 1965 1987 2001 1995 1963 1960 2003 ...
    ##  $ yr_renovated : int  0 1991 0 0 0 0 0 0 0 0 ...
    ##  $ zipcode      : int  98178 98125 98028 98136 98074 98053 98003 98198 98146 98038 ...
    ##  $ lat          : num  47.5 47.7 47.7 47.5 47.6 ...
    ##  $ long         : num  -122 -122 -122 -122 -122 ...
    ##  $ sqft_living15: int  1340 1690 2720 1360 1800 4760 2238 1650 1780 2390 ...
    ##  $ sqft_lot15   : int  5650 7639 8062 5000 7503 101930 6819 9711 8113 7570 ...

> Remove unwanted vaiables

``` r
house$id<-NULL
house$date<-NULL
```

> Check summary of data

``` r
summary(house)
```

    ##      price            bedrooms        bathrooms      sqft_living   
    ##  Min.   :  75000   Min.   : 0.000   Min.   :0.000   Min.   :  290  
    ##  1st Qu.: 321962   1st Qu.: 3.000   1st Qu.:1.750   1st Qu.: 1427  
    ##  Median : 450000   Median : 3.000   Median :2.250   Median : 1910  
    ##  Mean   : 540208   Mean   : 3.371   Mean   :2.115   Mean   : 2080  
    ##  3rd Qu.: 645000   3rd Qu.: 4.000   3rd Qu.:2.500   3rd Qu.: 2550  
    ##  Max.   :7700000   Max.   :33.000   Max.   :8.000   Max.   :13540  
    ##  NA's   :3                                                         
    ##     sqft_lot           floors        waterfront            view       
    ##  Min.   :    520   Min.   :1.000   Min.   :0.000000   Min.   :0.0000  
    ##  1st Qu.:   5040   1st Qu.:1.000   1st Qu.:0.000000   1st Qu.:0.0000  
    ##  Median :   7620   Median :1.500   Median :0.000000   Median :0.0000  
    ##  Mean   :  15108   Mean   :1.494   Mean   :0.007542   Mean   :0.2343  
    ##  3rd Qu.:  10688   3rd Qu.:2.000   3rd Qu.:0.000000   3rd Qu.:0.0000  
    ##  Max.   :1651359   Max.   :3.500   Max.   :1.000000   Max.   :4.0000  
    ##  NA's   :2         NA's   :4       NA's   :1                          
    ##    condition         grade          sqft_above   sqft_basement   
    ##  Min.   :1.000   Min.   : 1.000   Min.   : 290   Min.   :   0.0  
    ##  1st Qu.:3.000   1st Qu.: 7.000   1st Qu.:1190   1st Qu.:   0.0  
    ##  Median :3.000   Median : 7.000   Median :1560   Median :   0.0  
    ##  Mean   :3.409   Mean   : 7.657   Mean   :1788   Mean   : 291.5  
    ##  3rd Qu.:4.000   3rd Qu.: 8.000   3rd Qu.:2210   3rd Qu.: 560.0  
    ##  Max.   :5.000   Max.   :13.000   Max.   :9410   Max.   :4820.0  
    ##  NA's   :3                                                       
    ##     yr_built     yr_renovated       zipcode           lat       
    ##  Min.   :1900   Min.   :   0.0   Min.   :98001   Min.   :47.16  
    ##  1st Qu.:1951   1st Qu.:   0.0   1st Qu.:98033   1st Qu.:47.47  
    ##  Median :1975   Median :   0.0   Median :98065   Median :47.57  
    ##  Mean   :1971   Mean   :  84.4   Mean   :98078   Mean   :47.56  
    ##  3rd Qu.:1997   3rd Qu.:   0.0   3rd Qu.:98118   3rd Qu.:47.68  
    ##  Max.   :2015   Max.   :2015.0   Max.   :98199   Max.   :47.78  
    ##                                                                 
    ##       long        sqft_living15    sqft_lot15    
    ##  Min.   :-122.5   Min.   : 399   Min.   :   651  
    ##  1st Qu.:-122.3   1st Qu.:1490   1st Qu.:  5100  
    ##  Median :-122.2   Median :1840   Median :  7620  
    ##  Mean   :-122.2   Mean   :1987   Mean   : 12768  
    ##  3rd Qu.:-122.1   3rd Qu.:2360   3rd Qu.: 10083  
    ##  Max.   :-121.3   Max.   :6210   Max.   :871200  
    ## 

> Convert categorical variable into factor

``` r
vect1<-c("bedrooms","floors","waterfront","view","condition","grade","zipcode")
house[,vect1]<-lapply(house[,vect1],factor)
str(house)
```

    ## 'data.frame':    21613 obs. of  19 variables:
    ##  $ price        : num  221900 538000 180000 604000 510000 ...
    ##  $ bedrooms     : Factor w/ 13 levels "0","1","2","3",..: 4 4 3 5 4 5 4 4 4 4 ...
    ##  $ bathrooms    : num  1 2.25 1 3 2 4.5 2.25 1.5 1 2.5 ...
    ##  $ sqft_living  : int  1180 2570 770 1960 1680 5420 1715 1060 1780 1890 ...
    ##  $ sqft_lot     : int  5650 7242 10000 5000 8080 101930 NA 9711 7470 6560 ...
    ##  $ floors       : Factor w/ 6 levels "1","1.5","2",..: 1 3 1 1 1 1 3 1 1 3 ...
    ##  $ waterfront   : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ view         : Factor w/ 5 levels "0","1","2","3",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ condition    : Factor w/ 5 levels "1","2","3","4",..: 3 3 3 5 3 3 3 3 3 3 ...
    ##  $ grade        : Factor w/ 12 levels "1","3","4","5",..: 6 6 5 6 7 10 6 6 6 6 ...
    ##  $ sqft_above   : int  1180 2170 770 1050 1680 3890 1715 1060 1050 1890 ...
    ##  $ sqft_basement: int  0 400 0 910 0 1530 0 0 730 0 ...
    ##  $ yr_built     : int  1955 1951 1933 1965 1987 2001 1995 1963 1960 2003 ...
    ##  $ yr_renovated : int  0 1991 0 0 0 0 0 0 0 0 ...
    ##  $ zipcode      : Factor w/ 70 levels "98001","98002",..: 67 56 17 59 38 30 3 69 61 24 ...
    ##  $ lat          : num  47.5 47.7 47.7 47.5 47.6 ...
    ##  $ long         : num  -122 -122 -122 -122 -122 ...
    ##  $ sqft_living15: int  1340 1690 2720 1360 1800 4760 2238 1650 1780 2390 ...
    ##  $ sqft_lot15   : int  5650 7639 8062 5000 7503 101930 6819 9711 8113 7570 ...

> Check any NA/blank value in data set, if present then impute

``` r
colSums(is.na(house))
```

    ##         price      bedrooms     bathrooms   sqft_living      sqft_lot 
    ##             3             0             0             0             2 
    ##        floors    waterfront          view     condition         grade 
    ##             4             1             0             3             0 
    ##    sqft_above sqft_basement      yr_built  yr_renovated       zipcode 
    ##             0             0             0             0             0 
    ##           lat          long sqft_living15    sqft_lot15 
    ##             0             0             0             0

``` r
house$price[is.na(house$price)]<-median(house$price,na.rm = T)

house$sqft_lot[is.na(house$sqft_lot)]<-median(house$sqft_lot, na.rm = T)

summary(house$waterfront)
```

    ##     0     1  NA's 
    ## 21449   163     1

``` r
house$waterfront[is.na(house$waterfront)]<-"0"

summary(house$condition)
```

    ##     1     2     3     4     5  NA's 
    ##    30   172 14030  5677  1701     3

``` r
house$condition[is.na(house$condition)]<-"3"

summary(house$floors)
```

    ##     1   1.5     2   2.5     3   3.5  NA's 
    ## 10677  1909  8241   161   613     8     4

``` r
house$floors[is.na(house$floors)]<-"1"

colSums(is.na(house))
```

    ##         price      bedrooms     bathrooms   sqft_living      sqft_lot 
    ##             0             0             0             0             0 
    ##        floors    waterfront          view     condition         grade 
    ##             0             0             0             0             0 
    ##    sqft_above sqft_basement      yr_built  yr_renovated       zipcode 
    ##             0             0             0             0             0 
    ##           lat          long sqft_living15    sqft_lot15 
    ##             0             0             0             0

> Calculate count of years when house was build and renovated

``` r
house$yr_built<-2020 - house$yr_built
house$yr_renovated<- 2020 - house$yr_renovated
house$yr_renovated[house$yr_renovated==2020]<-0
```

> Remove outliers from dependent variable  
> Dependent variable : Price  
> Create boxplot of price variable

``` r
boxplot(house$price)
```

![](../doc/Data-science-with-R/outlier-1.png?raw=true)

``` r
#UW<-Q3+1.5*IQR
summary(house$price)#Q3=645000
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   75000  322000  450000  540195  645000 7700000

``` r
UW<-645000+(1.5*(IQR(house$price)));UW
```

    ## [1] 1129500

``` r
house_data<-subset(house,house$price<=UW)
head(house_data)
```

    ##    price bedrooms bathrooms sqft_living sqft_lot floors waterfront view
    ## 1 221900        3      1.00        1180     5650      1          0    0
    ## 2 538000        3      2.25        2570     7242      2          0    0
    ## 3 180000        2      1.00         770    10000      1          0    0
    ## 4 604000        4      3.00        1960     5000      1          0    0
    ## 5 510000        3      2.00        1680     8080      1          0    0
    ## 7 257500        3      2.25        1715     7620      2          0    0
    ##   condition grade sqft_above sqft_basement yr_built yr_renovated zipcode
    ## 1         3     7       1180             0       65            0   98178
    ## 2         3     7       2170           400       69           29   98125
    ## 3         3     6        770             0       87            0   98028
    ## 4         5     7       1050           910       55            0   98136
    ## 5         3     8       1680             0       33            0   98074
    ## 7         3     7       1715             0       25            0   98003
    ##       lat     long sqft_living15 sqft_lot15
    ## 1 47.5112 -122.257          1340       5650
    ## 2 47.7210 -122.319          1690       7639
    ## 3 47.7379 -122.233          2720       8062
    ## 4 47.5208 -122.393          1360       5000
    ## 5 47.6168 -122.045          1800       7503
    ## 7 47.3097 -122.327          2238       6819

> Zipcode is factor variable but have more than 15 levels.  
> Factor should consist of max 15 levels. Dropping Zipcode variable

``` r
levels(house_data$zipcode)
```

    ##  [1] "98001" "98002" "98003" "98004" "98005" "98006" "98007" "98008" "98010"
    ## [10] "98011" "98014" "98019" "98022" "98023" "98024" "98027" "98028" "98029"
    ## [19] "98030" "98031" "98032" "98033" "98034" "98038" "98039" "98040" "98042"
    ## [28] "98045" "98052" "98053" "98055" "98056" "98058" "98059" "98065" "98070"
    ## [37] "98072" "98074" "98075" "98077" "98092" "98102" "98103" "98105" "98106"
    ## [46] "98107" "98108" "98109" "98112" "98115" "98116" "98117" "98118" "98119"
    ## [55] "98122" "98125" "98126" "98133" "98136" "98144" "98146" "98148" "98155"
    ## [64] "98166" "98168" "98177" "98178" "98188" "98198" "98199"

``` r
house_data$zipcode<-NULL
```

> Reducing levels of bedroom

``` r
dim(house_data)
```

    ## [1] 20454    18

``` r
summary(house_data$bedrooms)
```

    ##    0    1    2    3    4    5    6    7    8    9   10   11   33 
    ##   12  198 2736 9597 6304 1335  225   30    9    4    2    1    1

``` r
house_data$bedrooms<-as.numeric(house_data$bedrooms)
house_data<-subset(house_data,house_data$bedrooms<=6)
house_data$bedrooms<-as.factor(house_data$bedrooms)
```

> Cross validation  
> Dividing data in 75-25\[training(75)- testing(25)\]

``` r
#install.packages("caret") ----install package if not installed
library(caret)
```

    ## Warning: package 'caret' was built under R version 3.6.3

    ## Loading required package: lattice

    ## Loading required package: ggplot2

    ## Warning: package 'ggplot2' was built under R version 3.6.3

``` r
set.seed(123)
index<-createDataPartition(house_data$floors,p=0.75,list = F)
head(index)
```

    ##      Resample1
    ## [1,]         1
    ## [2,]         2
    ## [3,]         3
    ## [4,]         5
    ## [5,]         6
    ## [6,]         7

``` r
training_house<-house_data[index,]
head(training_house)
```

    ##    price bedrooms bathrooms sqft_living sqft_lot floors waterfront view
    ## 1 221900        4      1.00        1180     5650      1          0    0
    ## 2 538000        4      2.25        2570     7242      2          0    0
    ## 3 180000        3      1.00         770    10000      1          0    0
    ## 5 510000        4      2.00        1680     8080      1          0    0
    ## 7 257500        4      2.25        1715     7620      2          0    0
    ## 8 291850        4      1.50        1060     9711      1          0    0
    ##   condition grade sqft_above sqft_basement yr_built yr_renovated     lat
    ## 1         3     7       1180             0       65            0 47.5112
    ## 2         3     7       2170           400       69           29 47.7210
    ## 3         3     6        770             0       87            0 47.7379
    ## 5         3     8       1680             0       33            0 47.6168
    ## 7         3     7       1715             0       25            0 47.3097
    ## 8         3     7       1060             0       57            0 47.4095
    ##       long sqft_living15 sqft_lot15
    ## 1 -122.257          1340       5650
    ## 2 -122.319          1690       7639
    ## 3 -122.233          2720       8062
    ## 5 -122.045          1800       7503
    ## 7 -122.327          2238       6819
    ## 8 -122.315          1650       9711

``` r
testing_house<-house_data[-index,]
```

> Model selection method

``` r
# full model
house_model_full<-lm(price~.,data = training_house)
summary(house_model_full)
```

    ## 
    ## Call:
    ## lm(formula = price ~ ., data = training_house)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -556183  -72892   -7676   60737  675267 
    ## 
    ## Coefficients: (1 not defined because of singularities)
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   -2.630e+07  1.051e+06 -25.021  < 2e-16 ***
    ## bedrooms2     -1.732e+04  4.211e+04  -0.411 0.680776    
    ## bedrooms3      4.154e+03  4.113e+04   0.101 0.919541    
    ## bedrooms4     -1.228e+04  4.107e+04  -0.299 0.764888    
    ## bedrooms5     -1.907e+04  4.111e+04  -0.464 0.642768    
    ## bedrooms6     -3.164e+04  4.127e+04  -0.767 0.443337    
    ## bathrooms      2.872e+04  2.335e+03  12.303  < 2e-16 ***
    ## sqft_living    6.725e+01  3.283e+00  20.481  < 2e-16 ***
    ## sqft_lot       2.661e-01  3.553e-02   7.488 7.36e-14 ***
    ## floors1.5      2.838e+04  3.708e+03   7.655 2.06e-14 ***
    ## floors2        2.242e+04  3.074e+03   7.295 3.15e-13 ***
    ## floors2.5      6.478e+04  1.323e+04   4.895 9.91e-07 ***
    ## floors3        5.310e+04  6.622e+03   8.018 1.15e-15 ***
    ## floors3.5      1.462e+05  4.729e+04   3.091 0.001995 ** 
    ## waterfront1    1.123e+05  1.995e+04   5.631 1.83e-08 ***
    ## view1          7.360e+04  7.948e+03   9.260  < 2e-16 ***
    ## view2          6.083e+04  4.980e+03  12.213  < 2e-16 ***
    ## view3          7.961e+04  7.458e+03  10.675  < 2e-16 ***
    ## view4          1.226e+05  1.266e+04   9.680  < 2e-16 ***
    ## condition2     4.371e+04  2.613e+04   1.673 0.094394 .  
    ## condition3     6.959e+04  2.416e+04   2.880 0.003981 ** 
    ## condition4     9.613e+04  2.417e+04   3.977 7.01e-05 ***
    ## condition5     1.227e+05  2.434e+04   5.041 4.68e-07 ***
    ## grade3         2.132e+04  1.687e+05   0.126 0.899434    
    ## grade4        -7.829e+04  1.255e+05  -0.624 0.532621    
    ## grade5        -8.117e+04  1.239e+05  -0.655 0.512218    
    ## grade6        -5.230e+04  1.238e+05  -0.422 0.672702    
    ## grade7         1.203e+04  1.238e+05   0.097 0.922567    
    ## grade8         8.875e+04  1.239e+05   0.717 0.473657    
    ## grade9         1.854e+05  1.239e+05   1.496 0.134687    
    ## grade10        2.390e+05  1.241e+05   1.927 0.054042 .  
    ## grade11        2.802e+05  1.246e+05   2.249 0.024503 *  
    ## grade12        2.432e+05  1.368e+05   1.778 0.075391 .  
    ## sqft_above     1.139e+00  3.349e+00   0.340 0.733807    
    ## sqft_basement         NA         NA      NA       NA    
    ## yr_built       1.715e+03  5.545e+01  30.920  < 2e-16 ***
    ## yr_renovated   8.375e+01  1.792e+02   0.467 0.640329    
    ## lat            5.269e+05  7.080e+03  74.412  < 2e-16 ***
    ## long          -9.852e+03  8.103e+03  -1.216 0.224066    
    ## sqft_living15  4.996e+01  2.574e+00  19.415  < 2e-16 ***
    ## sqft_lot15    -1.965e-01  5.226e-02  -3.760 0.000171 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 114200 on 15100 degrees of freedom
    ## Multiple R-squared:  0.6993, Adjusted R-squared:  0.6985 
    ## F-statistic: 900.4 on 39 and 15100 DF,  p-value: < 2.2e-16

``` r
# null model 
house_model_null<-lm(price~1,data = training_house)

# Stepwise forward regression
step(house_model_null,direction = "forward",
     scope = list(lower=house_model_null,upper=house_model_full))
```

    ## Start:  AIC=370783.5
    ## price ~ 1
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + grade         10 2.6815e+14 3.8661e+14 362827
    ## + sqft_living    1 2.5332e+14 4.0144e+14 363379
    ## + sqft_living15  1 2.1201e+14 4.4275e+14 364862
    ## + sqft_above     1 1.8491e+14 4.6985e+14 365761
    ## + bathrooms      1 1.3582e+14 5.1894e+14 367266
    ## + lat            1 1.2290e+14 5.3186e+14 367638
    ## + bedrooms       5 6.3289e+13 5.9147e+14 369254
    ## + floors         5 6.0959e+13 5.9380e+14 369314
    ## + view           4 4.0280e+13 6.1448e+14 369830
    ## + sqft_basement  1 3.3888e+13 6.2087e+14 369981
    ## + condition      4 7.1424e+12 6.4762e+14 370625
    ## + sqft_lot       1 6.7389e+12 6.4802e+14 370629
    ## + sqft_lot15     1 5.6540e+12 6.4910e+14 370654
    ## + long           1 4.0115e+12 6.5075e+14 370692
    ## + yr_built       1 2.9349e+12 6.5182e+14 370717
    ## + waterfront     1 1.4687e+12 6.5329e+14 370752
    ## + yr_renovated   1 1.0200e+12 6.5374e+14 370762
    ## <none>                        6.5476e+14 370784
    ## 
    ## Step:  AIC=362827.1
    ## price ~ grade
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + lat            1 9.4027e+13 2.9258e+14 358610
    ## + yr_built       1 5.3267e+13 3.3334e+14 360585
    ## + sqft_living    1 3.5928e+13 3.5068e+14 361352
    ## + sqft_basement  1 2.4945e+13 3.6167e+14 361819
    ## + sqft_living15  1 2.0896e+13 3.6572e+14 361988
    ## + view           4 1.8145e+13 3.6847e+14 362107
    ## + floors         5 1.4990e+13 3.7162e+14 362238
    ## + condition      4 1.4763e+13 3.7185e+14 362246
    ## + bedrooms       5 8.4663e+12 3.7815e+14 362502
    ## + sqft_above     1 6.0213e+12 3.8059e+14 362591
    ## + bathrooms      1 4.4698e+12 3.8214e+14 362653
    ## + long           1 4.1380e+12 3.8247e+14 362666
    ## + yr_renovated   1 2.9982e+12 3.8361e+14 362711
    ## + waterfront     1 2.2108e+12 3.8440e+14 362742
    ## + sqft_lot       1 6.4045e+11 3.8597e+14 362804
    ## + sqft_lot15     1 1.5217e+11 3.8646e+14 362823
    ## <none>                        3.8661e+14 362827
    ## 
    ## Step:  AIC=358610
    ## price ~ grade + lat
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + sqft_living    1 4.5109e+13 2.4748e+14 356077
    ## + yr_built       1 2.6971e+13 2.6561e+14 357148
    ## + sqft_living15  1 2.5363e+13 2.6722e+14 357239
    ## + view           4 2.0198e+13 2.7239e+14 357535
    ## + sqft_basement  1 1.6889e+13 2.7569e+14 357712
    ## + sqft_above     1 1.5846e+13 2.7674e+14 357769
    ## + condition      4 1.5149e+13 2.7743e+14 357813
    ## + floors         5 1.1023e+13 2.8156e+14 358039
    ## + bedrooms       5 1.0459e+13 2.8212e+14 358069
    ## + bathrooms      1 8.9078e+12 2.8368e+14 358144
    ## + waterfront     1 3.5102e+12 2.8907e+14 358429
    ## + sqft_lot       1 3.3287e+12 2.8926e+14 358439
    ## + yr_renovated   1 2.4262e+12 2.9016e+14 358486
    ## + sqft_lot15     1 1.8029e+12 2.9078e+14 358518
    ## + long           1 3.1313e+11 2.9227e+14 358596
    ## <none>                        2.9258e+14 358610
    ## 
    ## Step:  AIC=356077
    ## price ~ grade + lat + sqft_living
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + yr_built       1 2.7585e+13 2.1989e+14 354290
    ## + view           4 1.4496e+13 2.3298e+14 355171
    ## + condition      4 1.1863e+13 2.3561e+14 355341
    ## + floors         5 7.5615e+12 2.3991e+14 355617
    ## + sqft_living15  1 3.7088e+12 2.4377e+14 355850
    ## + waterfront     1 3.2751e+12 2.4420e+14 355877
    ## + long           1 3.1563e+12 2.4432e+14 355885
    ## + yr_renovated   1 1.6524e+12 2.4582e+14 355978
    ## + sqft_above     1 1.1153e+12 2.4636e+14 356011
    ## + sqft_basement  1 1.1153e+12 2.4636e+14 356011
    ## + sqft_lot       1 9.4885e+11 2.4653e+14 356021
    ## + bedrooms       5 6.5136e+11 2.4682e+14 356047
    ## + bathrooms      1 2.0277e+11 2.4727e+14 356067
    ## + sqft_lot15     1 1.5978e+11 2.4732e+14 356069
    ## <none>                        2.4748e+14 356077
    ## 
    ## Step:  AIC=354289.7
    ## price ~ grade + lat + sqft_living + yr_built
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + view           4 8.5797e+12 2.1131e+14 353695
    ## + sqft_living15  1 5.0362e+12 2.1485e+14 353941
    ## + condition      4 3.1308e+12 2.1676e+14 354081
    ## + bathrooms      1 3.0059e+12 2.1688e+14 354083
    ## + waterfront     1 2.3571e+12 2.1753e+14 354129
    ## + floors         5 1.7488e+12 2.1814e+14 354179
    ## + sqft_lot       1 7.2769e+11 2.1916e+14 354241
    ## + bedrooms       5 5.1655e+11 2.1937e+14 354264
    ## + sqft_lot15     1 1.4640e+11 2.1974e+14 354282
    ## + sqft_above     1 8.4323e+10 2.1981e+14 354286
    ## + sqft_basement  1 8.4323e+10 2.1981e+14 354286
    ## + yr_renovated   1 3.3809e+10 2.1986e+14 354289
    ## <none>                        2.1989e+14 354290
    ## + long           1 1.5386e+08 2.1989e+14 354292
    ## 
    ## Step:  AIC=353695.1
    ## price ~ grade + lat + sqft_living + yr_built + view
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + sqft_living15  1 3.7674e+12 2.0754e+14 353425
    ## + condition      4 3.2136e+12 2.0810e+14 353471
    ## + bathrooms      1 2.6700e+12 2.0864e+14 353505
    ## + floors         5 2.0334e+12 2.0928e+14 353559
    ## + sqft_lot       1 5.5781e+11 2.1075e+14 353657
    ## + sqft_above     1 4.9410e+11 2.1082e+14 353662
    ## + sqft_basement  1 4.9410e+11 2.1082e+14 353662
    ## + waterfront     1 4.8881e+11 2.1082e+14 353662
    ## + bedrooms       5 3.1004e+11 2.1100e+14 353683
    ## + long           1 9.8604e+10 2.1121e+14 353690
    ## + sqft_lot15     1 7.9862e+10 2.1123e+14 353691
    ## <none>                        2.1131e+14 353695
    ## + yr_renovated   1 8.7033e+09 2.1130e+14 353696
    ## 
    ## Step:  AIC=353424.8
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + condition      4 3.4059e+12 2.0414e+14 353182
    ## + bathrooms      1 3.1285e+12 2.0441e+14 353197
    ## + floors         5 2.8268e+12 2.0472e+14 353227
    ## + waterfront     1 5.1627e+11 2.0703e+14 353389
    ## + sqft_lot       1 4.6744e+11 2.0708e+14 353393
    ## + bedrooms       5 3.3248e+11 2.0721e+14 353410
    ## + sqft_above     1 1.6564e+11 2.0738e+14 353415
    ## + sqft_basement  1 1.6564e+11 2.0738e+14 353415
    ## <none>                        2.0754e+14 353425
    ## + sqft_lot15     1 2.5687e+10 2.0752e+14 353425
    ## + long           1 2.3573e+10 2.0752e+14 353425
    ## + yr_renovated   1 2.2252e+10 2.0752e+14 353425
    ## 
    ## Step:  AIC=353182.2
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15 + 
    ##     condition
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + floors         5 3.4975e+12 2.0064e+14 352931
    ## + bathrooms      1 2.7411e+12 2.0140e+14 352980
    ## + waterfront     1 5.3479e+11 2.0360e+14 353145
    ## + sqft_lot       1 4.6235e+11 2.0367e+14 353150
    ## + sqft_above     1 3.3989e+11 2.0380e+14 353159
    ## + sqft_basement  1 3.3989e+11 2.0380e+14 353159
    ## + bedrooms       5 4.1776e+11 2.0372e+14 353161
    ## + yr_renovated   1 1.0167e+11 2.0404e+14 353177
    ## + long           1 5.9118e+10 2.0408e+14 353180
    ## <none>                        2.0414e+14 353182
    ## + sqft_lot15     1 2.2517e+10 2.0411e+14 353183
    ## 
    ## Step:  AIC=352930.6
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15 + 
    ##     condition + floors
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + bathrooms      1 1.7268e+12 1.9891e+14 352802
    ## + sqft_lot       1 6.0167e+11 2.0004e+14 352887
    ## + waterfront     1 4.6409e+11 2.0018e+14 352898
    ## + bedrooms       5 4.5585e+11 2.0018e+14 352906
    ## + sqft_lot15     1 6.8527e+10 2.0057e+14 352927
    ## + yr_renovated   1 3.9945e+10 2.0060e+14 352930
    ## + sqft_above     1 3.2162e+10 2.0061e+14 352930
    ## + sqft_basement  1 3.2162e+10 2.0061e+14 352930
    ## <none>                        2.0064e+14 352931
    ## + long           1 1.0224e+10 2.0063e+14 352932
    ## 
    ## Step:  AIC=352801.7
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15 + 
    ##     condition + floors + bathrooms
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + bedrooms       5 8.1197e+11 1.9810e+14 352750
    ## + sqft_lot       1 6.6846e+11 1.9824e+14 352753
    ## + waterfront     1 4.5039e+11 1.9846e+14 352769
    ## + sqft_lot15     1 1.0403e+11 1.9881e+14 352796
    ## <none>                        1.9891e+14 352802
    ## + yr_renovated   1 1.2722e+10 1.9890e+14 352803
    ## + sqft_above     1 5.1078e+09 1.9891e+14 352803
    ## + sqft_basement  1 5.1078e+09 1.9891e+14 352803
    ## + long           1 1.5484e+09 1.9891e+14 352804
    ## 
    ## Step:  AIC=352749.8
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15 + 
    ##     condition + floors + bathrooms + bedrooms
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + sqft_lot       1 5.8366e+11 1.9752e+14 352707
    ## + waterfront     1 4.1112e+11 1.9769e+14 352720
    ## + sqft_lot15     1 6.4090e+10 1.9804e+14 352747
    ## <none>                        1.9810e+14 352750
    ## + sqft_above     1 7.7501e+09 1.9809e+14 352751
    ## + sqft_basement  1 7.7501e+09 1.9809e+14 352751
    ## + yr_renovated   1 7.7110e+09 1.9809e+14 352751
    ## + long           1 8.0364e+08 1.9810e+14 352752
    ## 
    ## Step:  AIC=352707.1
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15 + 
    ##     condition + floors + bathrooms + bedrooms + sqft_lot
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + waterfront     1 4.1156e+11 1.9711e+14 352678
    ## + sqft_lot15     1 1.8029e+11 1.9734e+14 352695
    ## + long           1 3.3676e+10 1.9748e+14 352707
    ## <none>                        1.9752e+14 352707
    ## + yr_renovated   1 4.4917e+09 1.9751e+14 352709
    ## + sqft_above     1 8.6135e+08 1.9752e+14 352709
    ## + sqft_basement  1 8.6135e+08 1.9752e+14 352709
    ## 
    ## Step:  AIC=352677.5
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15 + 
    ##     condition + floors + bathrooms + bedrooms + sqft_lot + waterfront
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## + sqft_lot15     1 1.9413e+11 1.9691e+14 352665
    ## + long           1 2.8101e+10 1.9708e+14 352677
    ## <none>                        1.9711e+14 352678
    ## + yr_renovated   1 1.3007e+09 1.9710e+14 352679
    ## + sqft_above     1 4.0861e+07 1.9711e+14 352680
    ## + sqft_basement  1 4.0861e+07 1.9711e+14 352680
    ## 
    ## Step:  AIC=352664.6
    ## price ~ grade + lat + sqft_living + yr_built + view + sqft_living15 + 
    ##     condition + floors + bathrooms + bedrooms + sqft_lot + waterfront + 
    ##     sqft_lot15
    ## 
    ##                 Df  Sum of Sq        RSS    AIC
    ## <none>                        1.9691e+14 352665
    ## + long           1 1.7212e+10 1.9689e+14 352665
    ## + yr_renovated   1 2.1702e+09 1.9691e+14 352666
    ## + sqft_above     1 2.8653e+08 1.9691e+14 352667
    ## + sqft_basement  1 2.8653e+08 1.9691e+14 352667

    ## 
    ## Call:
    ## lm(formula = price ~ grade + lat + sqft_living + yr_built + view + 
    ##     sqft_living15 + condition + floors + bathrooms + bedrooms + 
    ##     sqft_lot + waterfront + sqft_lot15, data = training_house)
    ## 
    ## Coefficients:
    ##   (Intercept)         grade3         grade4         grade5         grade6  
    ##    -2.509e+07      2.136e+04     -7.634e+04     -7.876e+04     -4.920e+04  
    ##        grade7         grade8         grade9        grade10        grade11  
    ##     1.544e+04      9.243e+04      1.894e+05      2.432e+05      2.848e+05  
    ##       grade12            lat    sqft_living       yr_built          view1  
    ##     2.485e+05      5.266e+05      6.788e+01      1.739e+03      7.428e+04  
    ##         view2          view3          view4  sqft_living15     condition2  
    ##     6.108e+04      8.006e+04      1.228e+05      4.946e+01      4.357e+04  
    ##    condition3     condition4     condition5      floors1.5        floors2  
    ##     6.955e+04      9.587e+04      1.223e+05      2.846e+04      2.294e+04  
    ##     floors2.5        floors3      floors3.5      bathrooms      bedrooms2  
    ##     6.547e+04      5.492e+04      1.482e+05      2.877e+04     -1.702e+04  
    ##     bedrooms3      bedrooms4      bedrooms5      bedrooms6       sqft_lot  
    ##     4.630e+03     -1.200e+04     -1.875e+04     -3.130e+04      2.630e-01  
    ##   waterfront1     sqft_lot15  
    ##     1.137e+05     -2.009e-01

``` r
#creating final model
house_model<-lm(formula = price ~ grade + lat + sqft_living + yr_built + view + 
    sqft_living15 + condition + floors + bathrooms + bedrooms + 
    sqft_lot + waterfront + sqft_lot15, data = training_house)

summary(house_model)
```

    ## 
    ## Call:
    ## lm(formula = price ~ grade + lat + sqft_living + yr_built + view + 
    ##     sqft_living15 + condition + floors + bathrooms + bedrooms + 
    ##     sqft_lot + waterfront + sqft_lot15, data = training_house)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -556086  -72953   -7871   60478  675842 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   -2.509e+07  3.528e+05 -71.114  < 2e-16 ***
    ## grade3         2.136e+04  1.687e+05   0.127 0.899256    
    ## grade4        -7.634e+04  1.254e+05  -0.609 0.542817    
    ## grade5        -7.876e+04  1.238e+05  -0.636 0.524742    
    ## grade6        -4.920e+04  1.238e+05  -0.398 0.690962    
    ## grade7         1.544e+04  1.238e+05   0.125 0.900701    
    ## grade8         9.243e+04  1.238e+05   0.747 0.455337    
    ## grade9         1.894e+05  1.239e+05   1.529 0.126366    
    ## grade10        2.432e+05  1.240e+05   1.961 0.049866 *  
    ## grade11        2.848e+05  1.245e+05   2.287 0.022185 *  
    ## grade12        2.485e+05  1.367e+05   1.818 0.069068 .  
    ## lat            5.266e+05  7.038e+03  74.818  < 2e-16 ***
    ## sqft_living    6.788e+01  2.572e+00  26.390  < 2e-16 ***
    ## yr_built       1.739e+03  5.075e+01  34.275  < 2e-16 ***
    ## view1          7.428e+04  7.905e+03   9.397  < 2e-16 ***
    ## view2          6.108e+04  4.957e+03  12.321  < 2e-16 ***
    ## view3          8.006e+04  7.412e+03  10.800  < 2e-16 ***
    ## view4          1.228e+05  1.262e+04   9.728  < 2e-16 ***
    ## sqft_living15  4.946e+01  2.489e+00  19.870  < 2e-16 ***
    ## condition2     4.357e+04  2.613e+04   1.668 0.095429 .  
    ## condition3     6.955e+04  2.416e+04   2.879 0.003996 ** 
    ## condition4     9.587e+04  2.417e+04   3.967 7.32e-05 ***
    ## condition5     1.223e+05  2.434e+04   5.025 5.09e-07 ***
    ## floors1.5      2.846e+04  3.609e+03   7.886 3.32e-15 ***
    ## floors2        2.294e+04  2.708e+03   8.471  < 2e-16 ***
    ## floors2.5      6.547e+04  1.314e+04   4.983 6.33e-07 ***
    ## floors3        5.492e+04  6.385e+03   8.602  < 2e-16 ***
    ## floors3.5      1.482e+05  4.724e+04   3.138 0.001707 ** 
    ## bathrooms      2.877e+04  2.285e+03  12.588  < 2e-16 ***
    ## bedrooms2     -1.702e+04  4.211e+04  -0.404 0.685989    
    ## bedrooms3      4.630e+03  4.112e+04   0.113 0.910352    
    ## bedrooms4     -1.200e+04  4.107e+04  -0.292 0.770098    
    ## bedrooms5     -1.875e+04  4.111e+04  -0.456 0.648299    
    ## bedrooms6     -3.130e+04  4.127e+04  -0.759 0.448128    
    ## sqft_lot       2.630e-01  3.540e-02   7.429 1.16e-13 ***
    ## waterfront1    1.137e+05  1.990e+04   5.712 1.14e-08 ***
    ## sqft_lot15    -2.009e-01  5.206e-02  -3.859 0.000114 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 114200 on 15103 degrees of freedom
    ## Multiple R-squared:  0.6993, Adjusted R-squared:  0.6985 
    ## F-statistic: 975.5 on 36 and 15103 DF,  p-value: < 2.2e-16

> check vif

``` r
#install.packages("car") ----install package if not installed
library(car)
```

    ## Warning: package 'car' was built under R version 3.6.3

    ## Loading required package: carData

    ## Warning: package 'carData' was built under R version 3.6.3

``` r
vif(house_model)
```

    ##                   GVIF Df GVIF^(1/(2*Df))
    ## grade         4.459844 10        1.077621
    ## lat           1.144430  1        1.069780
    ## sqft_living   4.481135  1        2.116869
    ## yr_built      2.528168  1        1.590021
    ## view          1.434746  4        1.046157
    ## sqft_living15 2.709592  1        1.646084
    ## condition     1.390768  4        1.042094
    ## floors        2.674036  5        1.103359
    ## bathrooms     2.963416  1        1.721458
    ## bedrooms      2.385772  5        1.090845
    ## sqft_lot      2.209206  1        1.486340
    ## waterfront    1.302575  1        1.141304
    ## sqft_lot15    2.245773  1        1.498590

> Predicting house price of training data set using house\_model

``` r
training_house$pred_price<-predict(house_model,training_house)
head(training_house)
```

    ##    price bedrooms bathrooms sqft_living sqft_lot floors waterfront view
    ## 1 221900        4      1.00        1180     5650      1          0    0
    ## 2 538000        4      2.25        2570     7242      2          0    0
    ## 3 180000        3      1.00         770    10000      1          0    0
    ## 5 510000        4      2.00        1680     8080      1          0    0
    ## 7 257500        4      2.25        1715     7620      2          0    0
    ## 8 291850        4      1.50        1060     9711      1          0    0
    ##   condition grade sqft_above sqft_basement yr_built yr_renovated     lat
    ## 1         3     7       1180             0       65            0 47.5112
    ## 2         3     7       2170           400       69           29 47.7210
    ## 3         3     6        770             0       87            0 47.7379
    ## 5         3     8       1680             0       33            0 47.6168
    ## 7         3     7       1715             0       25            0 47.3097
    ## 8         3     7       1060             0       57            0 47.4095
    ##       long sqft_living15 sqft_lot15 pred_price
    ## 1 -122.257          1340       5650   293367.4
    ## 2 -122.319          1690       7639   581385.8
    ## 3 -122.233          2720       8062   444089.4
    ## 5 -122.045          1800       7503   456029.9
    ## 7 -122.327          2238       6819   257592.7
    ## 8 -122.315          1650       9711   247719.7

> Calculate RMSE (Root Mean Squared Error) of training data

``` r
training_house$res<- residuals(house_model)
rmse_train<-sqrt(mean(training_house$res^2))
rmse_train
```

    ## [1] 114044.1

> Predicting house price of testing data set using house\_model

``` r
testing_house$pred_price<-predict(house_model,testing_house)
head(testing_house)
```

    ##     price bedrooms bathrooms sqft_living sqft_lot floors waterfront view
    ## 4  604000        5      3.00        1960     5000      1          0    0
    ## 9  229500        4      1.00        1780     7470      1          0    0
    ## 23 285000        6      2.50        2270     6300      2          0    0
    ## 27 937000        4      1.75        2450     2691      2          0    0
    ## 32 280000        3      1.50        1190     1265      3          0    0
    ## 36 696000        4      2.50        2300     3060    1.5          0    0
    ##    condition grade sqft_above sqft_basement yr_built yr_renovated     lat
    ## 4          5     7       1050           910       55            0 47.5208
    ## 9          3     7       1050           730       60            0 47.5123
    ## 23         3     8       2270             0       25            0 47.3266
    ## 27         3     8       1750           700      105            0 47.6386
    ## 32         3     7       1190             0       15            0 47.7274
    ## 36         3     8       1510           790       90           18 47.6827
    ##        long sqft_living15 sqft_lot15 pred_price
    ## 4  -122.393          1360       5000   438448.3
    ## 9  -122.337          1780       8113   347723.6
    ## 23 -122.169          2240       7005   368756.2
    ## 27 -122.360          1760       3573   658151.6
    ## 32 -122.357          1390       1756   408968.0
    ## 36 -122.310          1590       3264   663954.1

> Calculate RMSE (Root Mean Squared Error) of testing data

``` r
testing_house$res<- testing_house$price - testing_house$pred_price
rmse_test<-sqrt(mean(testing_house$res^2))
rmse_test
```

    ## [1] 112652

> check difference between rmse\_train and rmse\_test  
> The diff between rmse train and rmse test should not ecxeed by 15 %

``` r
diff_rmse<-rmse_train-rmse_test
diff_rmse
```

    ## [1] 1392.053

> durbinWatson Test

``` r
library(car)
durbinWatsonTest(house_model)
```

    ##  lag Autocorrelation D-W Statistic p-value
    ##    1      0.01500807      1.969929   0.078
    ##  Alternative hypothesis: rho != 0
