Incase you're wondering how to get the values from a password field like this:
![image](https://github.com/generalvimal/frappe_and_erpnext_documentations/assets/93727479/08659e6a-cc52-4438-b0db-601a9d8ceb3f)

it is not that hard, Just follow the instructions:

1. Go to the python console using ``` bench --site sitename console ```
2. Now get the document in which the password field is available using ``` doc = frappe.get_doc("Doctype Name", "document_name") ```
3. If it is a single doctype use ``` doc = frappe.get_doc("Doctype Name", "Doctype Name") ```
4. Now use ``` doc.get_password("password_field_name") ``` to get the password as text.

Done. Now you have the password as text.
