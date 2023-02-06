# Hospitality
Hospitality Dashboard Project Using MySQL and Power BI


create database hotel;
use hotel;

/*Sum revenue*/
select sum(revenue_realized) Revenue from fact_bookings;

/*Sum Occupancy*/
select (sum(successful_bookings)/sum(capacity))*100 Occupancy from fact_aggregated_bookings;

/*Sum Ratings*/
select avg(ratings_given) from fact_bookings;

/*City wise revenue*/
select city, sum(revenue_realized) Revenue from dim_hotels
left join fact_bookings on dim_hotels.property_id=fact_bookings.property_id group by city;

/*City wise occupancy*/
select city,(sum(successful_bookings)/sum(capacity))*100 Occupancy from dim_hotels
left join fact_aggregated_bookings on dim_hotels.property_id=fact_aggregated_bookings.property_id group by city;

/*City wise rating*/
select city, avg(ratings_given) Avg_rating from dim_hotels
left join fact_bookings on  dim_hotels.property_id=fact_bookings.property_id group by city;

/*City wise no of guest*/
select city,sum(no_guests) Total_guest from dim_hotels
left join fact_bookings on dim_hotels.property_id=fact_bookings.property_id group by city;

/*Line chart week,rating and occupancy*/
select week_no,(sum(successful_bookings)/sum(capacity))*100 Occupancy,avg(ratings_given) Avg_ratings from dim_date
left join fact_aggregated_bookings on dim_date.date=fact_aggregated_bookings.check_in_date
left join fact_bookings on dim_date.date=fact_bookings.check_in_date group by week_no order by week_no;

/*Veiw table*/
select property_name,avg(ratings_given),sum(revenue_realized),sum(no_guests),(sum(successful_bookings)/sum(capacity))*100 Occupancy  
from fact_bookings 
left join dim_hotels on fact_bookings.property_id=dim_hotels.property_id 
left join fact_aggregated_bookings on fact_bookings.property_id=fact_aggregated_bookings.property_id group by property_name;



