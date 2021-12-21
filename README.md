# DHL Datathon 2021

## Data Butchers - Winner

[![N|Solid](https://raw.githubusercontent.com/ihkaraman/ihkaraman/main/images/dhl_datathon_21.png)](https://www.linkedin.com/feed/update/urn:li:activity:6877662215583891456/)

In DHL Datathon 2021, there were 3 different projects; Product Segmentation, Volume Prediction, and Developing a Picking Algorithm. Detailed solution approaches to these projects are presented below.

### Product Segmentation

ABC analysis and the K-Means Clustering approach are used for segmenting products. With the limited and masked dataset, there aren’t available product features, therefore, volume info is used for ABC analysis, and total monthly average orders, monthly average order counts, and the total number of unique customers are used as features for the clustering approach.

### Volume Prediction

In the given dataset, the past order amounts are shared for specific customer groups for specific product families. Firstly, data is created for daily periods and to simplify the problem weekly period is used for modeling. In the feature engineering step, firstly, past data is prepared for each customer-product pair to see the customer’s behavior for the specific product. Secondly, trends for customer economy, product economy, and general economy are created with grouping operations. By creating these grouped features, we want to model customers, all the products, and the general economy’s past trends. In this way, the model is able to see the past trends for the customer’s orders, demand for products, and the whole economic condition. All the mentioned features are created for the past 2, 3, 4, 5, 7, 10, 14, 30 periods. 

Also, we used some additional datasets to enrich our feature set. To integrate economical conditions, USD/TRY, EUR/TRY exchange rates and their periodic fluctuation, producer price index, consumer price index, and their change rates are added. Lastly, a working calendar is added, since the expected order amounts are zero for non-working days under normal conditions. 

Using all the features, we tried different regression models ranging from simple to complex. After splitting the data using time-split cross-validation, the highest scores are obtained with XGBRegressor and LSTM. 

### Developing a Picking Algorithm

The picking algorithm is developed to collect the placed orders before the shipment date while minimizing the daily wave number under some capacity constraints. These constraints are: first, the daily picked amount cannot exceed a predefined capacity, second, in each wave, a limited number of types of material can be picked. After doing some research, using mathematical models is costly in terms of computation time and because of the time constraint in the operation process, we decided to move on with a heuristic approach to solve the problem in a fast and efficient way. The pseudo-code for the algorithm:

<p><em>&nbsp;&nbsp;for each day, do:</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. add the newly placed orders and the orders that haven't shipped to checklist</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. move the items that should be picked that day from checklist to picking_list</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. pick all the items from picking_list</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.1 if the daily capacity is not exceeded pick the items as 'normal_picking'</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.2 if the daily capacity is exceeded pick the items as 'overwork_picking'</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4. If there are still empty places in the waves and daily capacity is not full, do extra picking (wave utilization)</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1 pick the same type of materials that are in the checklist </em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2 if there are still empty places in the waves pick the orders that have the closest shipping date (if there isn't expected order for upcoming days for these materials)</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5. If the daily capacity is still not full, do extra picking</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.0 control the checklist if there is any bottleneck in the upcoming days</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.1 if there is any bottleneck, do extra picking to solve this bottleneck</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6. print the results as work orders</em></p>

The average capacity utilization of our algorithm is **94%** and the average wave utilization is **99%.**

further improvements for our algorithm:

1. balance the workload between days
2. add holidays calendar
3. minimize the workload for non-working days
4. further predictions are included but can be used more
