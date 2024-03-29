[![PyPI version](https://badge.fury.io/py/abpower.svg)](https://badge.fury.io/py/abpower)

# abpower

`abpower` is a parser for the publicly-available data provided by [AESO](https://aeso.ca/) related to the power grid in Alberta. It consists of a package and a command-line utility.

## Background

During the summer of 2020 (yes, _that_ summer) I built a website named the **Alberta Power Dashboard** that gathered and displayed data from AESO. It was fairly buggy and eventually I stopped maintaining it.

This is an attempt to write a more robust parser than the original, with the possibility of bringing the website back at some point in the future - or at least providing a parser someone else can use.

## Installation

With `pip`:-

```shell
pip install abpower
```

With `poetry`:-

```shell
poetry add abpower
```

## Usage
### The `abpower` package

You can query for all data currently supported by the module (see below) with the following:-

```python
from abpower import ETSParser

parser = ETSParser()
data = parser.get()
```

This will return an `ETS` object that contains the data. The `as_dict` and `as_json` properties will return `dict` and JSON string representations of the data respectively.

#### Querying specific data

You can pass a `list` or `tuple` of strings to the parser to only get and parse specific sections of the AESO data.

For example, to only query for the _Current Supply Demand_ and _System Marginal Price_ data:-

```python
from abpower import ETSParser

parser = ETSParser()
data = parser.get(query=["current-supply-demand", "system-marginal-price"])
```

You can also import the specific parser directly:-

```python
from abpower.parser import CurrentSupplyDemandParser, SystemMarginalPriceParser

csd_parser = CurrentSupplyDemandParser()
csd_data = csd_parser.get()

smp_parser = SystemMarginalPriceParser()
smp_data = smp_parser.get()
```

### The `abpower` command-line utility

A command-line utility - also named `abpower` - will be installed along with the module.

As with the module, you can query for all data with:-

```shell
abpower get
```

This will query, parse and return all data in JSON format. You can use the `--write-to-file` (or `-w`) option to write the data to a file instead of standard output.

#### Querying specific data

Also like the module, you can query for specific data only:-

```shell
abpower get -q current-supply-demand -q system-marginal-price
```

## Available data

Not all the data provided on the AESO website is queried or parsed by `abpower`. This may change in the future, but right now the following are supported:-

* `current-supply-demand` - the [Current Supply Demand](http://ets.aeso.ca/ets_web/ip/Market/Reports/CSDReportServlet) report, which gives an overview of the grid
* `actual-forecast` - the [Actual / Forecast](http://ets.aeso.ca/ets_web/ip/Market/Reports/ActualForecastWMRQHReportServlet) report, which gives a historical comparison of the forecasted and actual usage of the grid over the last 24 hours
* `daily-average-pool-price` - the [Daily Average Pool Price](http://ets.aeso.ca/ets_web/ip/Market/Reports/DailyAveragePoolPriceReportServlet) report, which gives averages of the pool price over the last week
* `hourly-available-capability` - the [7 Day Hourly Available Capability](http://ets.aeso.ca/ets_web/ip/Market/Reports/SevenDaysHourlyAvailableCapabilityReportServlet) report, which gives a forecast of hourly availability over the next 7 days
* `pool-price` - the [Pool Price](http://ets.aeso.ca/ets_web/ip/Market/Reports/SMPriceReportServlet) report, which gives the historical pool prices over the last 24 hours
* `supply-surplus` - the [Supply Surplus](http://ets.aeso.ca/ets_web/ip/Market/Reports/SupplySurplusReportServlet) report, which gives the forecasted surplus status for the next 6 hours
* `system-marginal-price` - the [System Marginal Price](http://ets.aeso.ca/ets_web/ip/Market/Reports/CSMPriceReportServlet) report, which gives the historical price over the last few hours
* `peak-load-forecast` the [Peak Load Forecast](http://ets.aeso.ca/ets_web/ip/Market/Reports/MonthlyPeakLoadForecastReportServlet) report, which gives the forecasted peak load for the next 7 days

## Known issues and future plans

There are no known issues, but this was initially written in a weekend so make of that what you will.

* Documentation needs writing and/or generating
* Tests need writing
* Coverage of the data available needs expanding

## Credits and contributing

`abpower` is written and maintained by Andy Smith. Pull requests and bug reports are welcome!

## License

`abpower` is distributed under the [MIT License](https://choosealicense.com/licenses/mit/).