## Top 10 alerts

To monitor the 10 most triggered alerts over a period you can choose:

1. Go to **Dashboard**.
2. Click **Install some sample data**.
3. Click **Add data** for each of the 3, one by one.

![[customize-the-dashboard--fig-01--sample-data-added.png]]
*the three sample data sets once added ("when the 3 look like this")*

4. Repeat the first step and click **Create new dashboard**.
5. Choose **Pie**, and choose the source `wazuh-alerts-*`.
6. Name it as you like from **Custom label**. In the **Buckets** section click **Add**, then **Split slices**, then **Aggregation → Terms**. Set **Field** to `rule.description`, because the goal is to understand what the alerts are for.
7. Choose the number of alerts that suits you, click **Update**, then **Save**. You can find it at the top.

You get something that looks like this:

![[customize-the-dashboard--fig-02--top-10-alerts-pie.png]]
*the resulting pie chart*

## Total number of alerts

Choose **Metric**. Click **Update**, then **Save**. You can customize it further.

![[customize-the-dashboard--fig-03--alerts-by-country.png]]
*the number of alerts produced by country*

To show the alerts by country, repeat the steps to display the total number of alerts: click **Add bucket**, **Aggregation**, **Terms**, set **Field** to `GeoLocation.country_name` (or the city name, as you like), set the number that suits you, then **Update** and **Save**.

![[customize-the-dashboard--extra-14--country-name-bucket.png]]
*the bucket: Split group, Terms, field `GeoLocation.country_name`, order Alphabetical, size 10*

## Map

Go to **Dashboard** or **Visualization**, then **Create visualization**, then **Coordinate map**. Choose `wazuh-alerts-*`, click **Add bucket**, choose **Geo coordinates**, set **Aggregation** to **Geohash**, set **Field** to `GeoLocation.location`, then **Update** and **Save**. If you do not see anything, try changing the date.

## Save the dashboard

Finally, to save your dashboard, click **Save**, then name it.

![[customize-the-dashboard--fig-04--my-dashboard.png]]
*the finished dashboard*

## Related

[[Configure Email Alerts]] · [[Install Wazuh (Recommended)]]
