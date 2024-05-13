To add a filter inside a report filter in erpnext/frappe you have to first open the JS file of the report and then add something similar to the following below the respective filter:

NOTE: Filters are written in JS

Here I am adding a filter inside a report filter based on another report filter
```
{
			"fieldname": "test",
			"label": __("Test"),
			"fieldtype": "Link",
			"options": "Test",
			get_query() {
				if (!frappe.query_report.get_filter_value("customer")) return
				return {
					filters: {
						customer: frappe.query_report.get_filter_value("customer"),
					}
				}
			},
		},
```

Full code:
```
{
			"fieldname": "customer",
			"label": __("Customer"),
			"fieldtype": "Link",
			"options": "Customer"
		},
		{
			"fieldname": "test",
			"label": __("Test"),
			"fieldtype": "Link",
			"options": "Test",
			get_query() {
				if (!frappe.query_report.get_filter_value("customer")) return
				return {
					filters: {
						customer: frappe.query_report.get_filter_value("customer"),
					}
				}
			},
		},
```
Here the Test report filter will show Test documents based on the customer selected in "Customer" report filter


Another way of doing filter is by writing your own python query API:

```
{
			"fieldname": "sales_order",
			"label": __("Sales Order"),
			"fieldtype": "Link",
			"options": "Sales Order",
			get_query() {
				if (!frappe.query_report.get_filter_value("project_code")) return
				return {
					query: "skipper.api.sales_order",
					filters: {
						"project_code": frappe.query_report.get_filter_value("project_code"),
					}
				}
			},
		},
```

in query mention the location of the python file and in filters add the values you ant to send the python API

The python code needs to be written in a python file like:

```
@frappe.whitelist()
@frappe.validate_and_sanitize_search_inputs
def sales_order(doctype, txt, searchfield, start, page_len, filters) -> list:
	if not filters.get("project_code"):return []
	value = frappe.get_value("Project", filters.get("project_code"), "custom_contract_po_number")
	if value:
		return [[value]]
	else: 
		return []
```

here you should note that the values need to be returned as list of list.
