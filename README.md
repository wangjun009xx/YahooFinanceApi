# YahooFinanceApi
[![Build status](https://ci.appveyor.com/api/projects/status/138s6on1y0wnaxms?svg=true)](https://ci.appveyor.com/project/lppkarl/yahoofinanceapi)
[![NuGet](https://img.shields.io/nuget/vpre/:packageName.svg)](https://www.nuget.org/packages/YahooFinanceApi/)
[![NuGet](https://img.shields.io/nuget/dt/YahooFinanceApi.svg)](https://www.nuget.org/packages/YahooFinanceApi/)
[![license](https://img.shields.io/github/license/lppkarl/YahooFinanceApi.svg)](https://github.com/lppkarl/YahooFinanceApi/blob/master/LICENSE)

A handy Yahoo! Finance api wrapper, based on .NET Standard 2.0

## Features
* Get quotes
* Get historical data
* Get dividend data
* Get stock split data

## Notes
This library is intended for personal use only, any improper use of this library is not recommended.

## Install Note
Dependencies: NodaTime

## Supported Platforms
* .NET Core 2.0
* .NET framework 4.6.1 or above
* Xamarin.iOS
* Xamarin.Android
* Universal Windows Platform

## How To Install
You can find the package through Nuget

    PM> Install-Package YahooFinanceApi

## How To Use

### Add namespoace reference

    using YahooFinanceApi;

### Get stock quotes
    // You could query multiple symbols with multiple fields through the following steps:
    var securities = await Yahoo.Symbols("AAPL", "GOOG").Fields(Field.Symbol, Field.RegularMarketPrice, Field.FiftyTwoWeekHigh).QueryAsync();
    var aapl = securities["AAPL"];
    var price = aapl[Field.RegularMarketPrice] // or, you could use aapl.RegularMarketPrice directly for typed-value

### Supported fields for stock quote
||||||
|--|--|--|--|--|--|
| Language | QuoteType | QuoteSourceName | Currency | MarketState | RegularMarketPrice | 
| RegularMarketTime | RegularMarketChange | RegularMarketOpen | RegularMarketDayHigh | RegularMarketDayLow | RegularMarketVolume |
| ShortName | FiftyTwoWeekHighChange | FiftyTwoWeekHighChangePercent | FiftyTwoWeekLow | FiftyTwoWeekHigh | DividendDate |
| EarningsTimestamp | EarningsTimestampStart | EarningsTimestampEnd | TrailingAnnualDividendRate | TrailingPE | TrailingAnnualDividendYield | 
| EpsTrailingTwelveMonths | EpsForward | SharesOutstanding | BookValue | RegularMarketChangePercent | RegularMarketPreviousClose | 
| Bid | Ask | BidSize | AskSize | MessageBoardId | FullExchangeName | 
| LongName | FinancialCurrency | AverageDailyVolume3Month | AverageDailyVolume10Day | FiftyTwoWeekLowChange | FiftyTwoWeekLowChangePercent |
| TwoHundredDayAverageChangePercent | MarketCap | ForwardPE | PriceToBook | SourceInterval | ExchangeTimezoneName |
| ExchangeTimezoneShortName | Market | Exchange | ExchangeDataDelayedBy | PriceHint | FiftyDayAverage |
| FiftyDayAverageChange | FiftyDayAverageChangePercent | TwoHundredDayAverage | TwoHundredDayAverageChange | Tradeable | GmtOffSetMilliseconds |
| Symbol |||||

### Ignore invalid rows
    // Sometimes, yahoo returns broken rows for historical calls, you could decide if these invalid rows is ignored or not by the following statement
    Yahoo.IgnoreEmptyRows = true;

### Get historical data for a stock
    // You should be able to query data from various markets including US, HK, TW
    // The startTime & endTime here defaults to EST timezone
    var history = await Yahoo.GetHistoricalAsync("AAPL", new DateTime(2016, 1, 1), new DateTime(2016, 7, 1), Period.Daily);

    foreach (var candle in history)
    {
        Console.WriteLine($"DateTime: {candle.DateTime}, Open: {candle.Open}, High: {candle.High}, Low: {candle.Low}, Close: {candle.Close}, Volume: {candle.Volume}, AdjustedClose: {candle.AdjustedClose}");
    }

### Get dividend history for a stock
    // You should be able to query data from various markets including US, HK, TW
    var dividends = await Yahoo.GetDividendsAsync("AAPL", new DateTime(2016, 1, 1), new DateTime(2016, 7, 1));
    foreach (var candle in dividends)
    {
        Console.WriteLine($"DateTime: {candle.DateTime}, Dividend: {candle.Dividend}");
    }

### Get stock split history for a stock
    var splits = await Yahoo.GetSplitsAsync("AAPL", new DateTime(2014, 6, 8), new DateTime(2014, 6, 10));
    foreach (var s in splits)
    {
        Console.WriteLine($"DateTime: {s.DateTime}, AfterSplit: {s.AfterSplit}, BeforeSplit: {s.BeforeSplit}");
    }

### Powered by
* [Flurl](https://github.com/tmenier/Flurl) ([@tmenier](https://github.com/tmenier)) - A simple & elegant fluent-style REST api library 
