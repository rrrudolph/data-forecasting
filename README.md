# data-forecasting

This module uses economic data forecasts to create buy/sell scores for currencies.  Forex Factory has a great calendar with data from all of the countries whose currencies make up the bulk of volume in the foreign exchange market. 

![pasted image 0](https://user-images.githubusercontent.com/62268115/121748495-b5606800-cace-11eb-8023-7b610c9f0078.png)        
Source: https://www.forexfactory.com/calendar

Accessing the calendar directly with Python is not possible so I used the Google Sheets API as an intermediary. The API made it possible to easily fill a database with historical data, with only minimal finnesse required to handle throttling and bad requests. 

Once the data was cleaned, the next step was to derive a relevance score for upcoming events in the calendar.  I wanted to calculate the importance that a data forecast may have to large funds and banks, thereby getting an idea for potential trades in the near future.  The premise for this is that (historically at least) interest rates drive currency flows, central banks set interest rates, and data drives central bank decision making.  Therefore, if I could determine the real importance of a given forecast I might gain a small directional edge.

Using normalized data, I applied the following calculations to each event:
- Find the difference between Forecast and Previous
- Apply an importance weighting (inverting some numbers, eg unemployment)
- Calculate the accuracy of the event’s Forecast in predicting the Actual
- Calculate the trend of the event’s recent Actuals
- Calculate the trend of the currency’s recent Actuals

After running each calendar event through these calculations I am left with a single rating for each forecast.   
![Screenshot from 2021-06-11 16-12-40](https://user-images.githubusercontent.com/62268115/121749256-f9a03800-cacf-11eb-9682-95c76eb4b5c6.png)

As useful as that may seem, I’ve been trading long enough to know that even the best ideas can end up being worthless.  In order to get an idea of the validity of my calculations, I needed to see if the ratings correlated to price movement. This can’t be done directly though, because with currencies you are always looking at the value of one against another. For example, EUR/CAD will show the value of the Euro against the Canadian dollar, but in order to see how the Euro’s forecast ratings affected it I needed to isolate EUR. The only way to do this is to make a currency index where the Euro is compared to everything else. 

In making the indexes I didn’t weight them according to trade volume, I simply normalized against volatility so that currencies with large daily ranges didn't overshadow those with small ranges. Below is an example of the Australian Dollar index with some hand-drawn SR lines.
![aud](https://user-images.githubusercontent.com/62268115/121749571-7df2bb00-cad0-11eb-90d7-f9a6d6a4a42c.png)

Now that I had the custom forecast data and a way to create currency indexes, I could plot the forecasts in the volume section (blue histogram) of the chart.
(ignore the green line here, thats from the correlation scanner)
![USD](https://user-images.githubusercontent.com/62268115/121750417-f60db080-cad1-11eb-9a34-fd8245500a1b.png)



