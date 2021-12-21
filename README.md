# DHL Datathon 2021
## Data Butchers - Winner
<p><span style="font-weight: 400;">In DHL Datathon 2021, there were 3 different projects; Product Segmentation, Volume Prediction, and Developing a Picking Algorithm. Detailed solution approaches to these projects are presented below.</span></p>
#### Product Segmentation
<p><span style="font-weight: 400;">ABC analysis and the K-Means Clustering approach are used for segmenting products. With the limited and masked dataset, there aren&rsquo;t available product features, therefore, volume info is used for ABC analysis, and total monthly average orders, monthly average order counts, and the total number of unique customers are used as features for the clustering approach.</span></p>
#### Volume Prediction<
<p><span style="font-weight: 400;">In the given dataset, the past order amounts are shared for specific customer groups for specific product families. Firstly, data is created for daily periods and to simplify the problem weekly period is used for modeling. In the feature engineering step, firstly, past data is prepared for each customer-product pair to see the customer&rsquo;s behavior for the specific product. Secondly, trends for customer economy, product economy, and general economy are created with grouping operations. By creating these grouped features, we want to model customers, all the products, and the general economy&rsquo;s past trends. In this way, the model is able to see the past trends for the customer&rsquo;s orders, demand for products, and the whole economic condition. All the mentioned features are created for the past 2, 3, 4, 5, 7, 10, 14, 30 periods.&nbsp;</span></p>
<p><span style="font-weight: 400;">Also, we used some additional datasets to enrich our feature set. To integrate economical conditions, USD/TRY, EUR/TRY exchange rates and their periodic fluctuation, producer price index, consumer price index, and their change rates are added. Lastly, a working calendar is added, since the expected order amounts are zero for non-working days under normal conditions.&nbsp;</span></p>
<p><span style="font-weight: 400;">Using all the features, we tried different regression models ranging from simple to complex. After splitting the data using time-split cross-validation, the highest scores are obtained with XGBRegressor and LSTM.&nbsp;</span></p>
#### Developing a Picking Algorithm
<p><span style="font-weight: 400;">The picking algorithm is developed to collect the placed orders before the shipment date while minimizing the daily wave number under some capacity constraints. These constraints are: first, the daily picked amount cannot exceed a predefined capacity, second, in each wave, a limited number of types of material can be picked. After doing some research, using mathematical models is costly in terms of computation time and because of the time constraint in the operation process, we decided to move on with a heuristic approach to solve the problem in a fast and efficient way. The pseudo-code for the algorithm:</span></p>
<p><span style="font-weight: 400;">for each day, do:</span></p>
<ol>
<p><li><em>1. add the newly placed orders and the orders that haven&rsquo;t shipped to checklist</em></li></p>
<p><li><em>2. move the items that should be picked that day from checklist to picking_list</em></li></p>
<p><li><em>3. pick all the items from picking_list</em></li></p>
</ol>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.1 if the daily capacity is not exceeded, </em><em>pick the items as&nbsp; 'normal_picking'&nbsp;</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.2 if the daily capacity is exceeded, </em><em>pick the items as 'overwork_picking'</em></p>
<ol start="4">
<li><em> If there are still empty places in the waves and daily capacity is not full, do extra picking (wave utilization)</em></li>
</ol>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1 pick the same type of materials that are in the checklist&nbsp;</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2 if there are still empty places in the waves pick the orders that have the closest shipping date (if there isn&rsquo;t expected order for upcoming days for these materials)</em></p>
<ol start="5">
<li><em> If the daily capacity is still not full, do extra picking</em></li>
</ol>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.0 control the checklist if there is any bottleneck in the upcoming days</em></p>
<p><em>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.1 if there is any bottleneck, do extra picking to solve this bottleneck</em></p>
<ol start="6">
<li><em> print the results as work orders</em></li>
</ol>
<p><span style="font-weight: 400;">The average capacity utilization of our algorithm is 94% and the average wave utilization is 99%.</span></p>
<p><span style="font-weight: 400;">Further improvements for our algorithm:</span></p>
<ol>
<li><span style="font-weight: 400;"> Balance the workload between days</span></li>
<li><span style="font-weight: 400;">Add holidays calendar</span></li>
<li><span style="font-weight: 400;">Minimize the workload for non-working days</span></li>
<li><span style="font-weight: 400;">Further predictions are included but can be used more</span></li>
</ol>
