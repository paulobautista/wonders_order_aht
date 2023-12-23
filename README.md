# Order Component Affecting AHT

We know Average Handling Time AHT is crucial for  operational efficiency, and it has become the defacto catch-all metric for operations. Currently, the company has set average handling time targets that agents, and operations as a whole, are measured against for performance evaluation, and certification. 

However, there is little analysis when it comes to what actually drives AHT and how we can improve operational efficiency.  It is assumed that faster AHT is better, but there is little understanding to what AHT should be, how that might evolve, and the potential tradeoffs with mistakes.

This project is outlined on this [Confluence Page](https://wondersco.atlassian.net/wiki/pages/resumedraft.action?draftId=2163508066&draftShareId=9995aedd-3afe-44d9-9b26-1937fe4f3e78)

## Result

![Screenshot 2023-12-22 at 11 48 25 PM](https://github.com/paulobautista/wonders_order_aht/assets/63529937/ec37bb53-027c-4dbb-b4a6-8e6fb594c70a)


## Data Query 
```sql
select
o.customer_id, 
o.restaurant_id, 
c.operator_id,
u.location_code, 
u.full_name,
u.position_name, 
u.created_at_est,
o.order_payment_type,
o.order_type,
c.call_time_in_seconds,
c.conversation_total_sales_in_usd
from prod_wonders_lake.call_center.calls c 
join prod_wonders_lake.core.orders o on c.first_order_id = o.id
left join prod_wonders_lake.core.internal_users u on u.id = c.operator_id
where call_type = 'inbound_call'
and conversation_status = 'submitted'
order by o.created_at desc 
limit 200000
```
