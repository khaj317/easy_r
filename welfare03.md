나이와 월급의 관계를 파악해봅시다.
================
강화정
July 30, 2020

## 3\. 나이와 월급의 관계

비정규직이 많아지면서 안정된 직장에 취업하는 것도 어려워졌지만, 젊은 세대를 더욱 힘들게 하는 것은 세대 간 소득 격차가 심해서
사회가 불평등하게 느껴진다는 점입니다. 나이에 따라 월급이 어떻게 다른지 데이터 분석을 통해 알아보겠습니다.

### 분석 절차

#### 1\. 변수 검토하기

  - 나이 & 월급

<!-- end list -->

``` r
class(welfare$birth)
```

``` r
summary(welfare$birth)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1907    1946    1966    1968    1988    2014

``` r
qplot(welfare$birth)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](welfare03_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

#### 2\. 전처리

  - 나이에 따른 월급 평균표 만들기 & 그래프 만들기

태어난 연도는 1900\~2014 사이의 값을 지니고, 9999는 모름/무응답을 의미합니다.

``` r
summary(welfare$birth)
table(is.na(welfare$birth))
```

``` r
welfare$birth <- ifelse(welfare$birth == 9999, NA, welfare$birth)
table(is.na(welfare$birth))
```

    ## 
    ## FALSE 
    ## 16664

#### 3\. 파생변수 만들기 - 나이

태어난 연도 변수를 검토해 나이 변수를 만듭니다. 2015년에 조사가 진행되었으므로, 2015에서 태어난 연도를 뺀 후에 1을
더해 나이를 구합니다.

``` r
welfare$age <- 2015 - welfare$birth + 1
summary(welfare$age)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    2.00   28.00   50.00   48.43   70.00  109.00

``` r
qplot(welfare$age)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](welfare03_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

### 나이와 월급의 관계 분석하기

#### 1\. 나이에 따른 월급 평균표 만들기

``` r
age_income <- welfare %>%
  filter(!is.na(income)) %>% 
  group_by(age) %>%
  summarise(mean_income = mean(income))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
head(age_income)
```

#### 2\. 그래프 만들기

``` r
ggplot(data = age_income, aes(x = age, y = mean_income)) + geom_line()
```

![](welfare03_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->
