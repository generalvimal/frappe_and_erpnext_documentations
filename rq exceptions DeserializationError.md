**Error:** "exception": "rq.exceptions.DeserializationError"

```
### App Versions
{
	"erpnext": "15.8.1",
	"frappe": "15.23.0",
	"hrms": "15.9.0",
	"custom_app": "0.0.1"
}

### Route

List/RQ Job/List

### Traceback

Traceback (most recent call last):
  File "env/lib/python3.10/site-packages/rq/job.py", line 486, in _deserialize_data
    self._func_name, self._instance, self._args, self._kwargs = self.serializer.loads(self.data)
AttributeError: Can't get attribute 'fetch_attachment_from_dependent_task' on <module 'custom_app.api' from 'apps/custom_app/custom_app/api.py'>

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "apps/frappe/frappe/app.py", line 110, in application
    response = frappe.api.handle(request)
  File "apps/frappe/frappe/api/__init__.py", line 49, in handle
    data = endpoint(**arguments)
  File "apps/frappe/frappe/api/v1.py", line 36, in handle_rpc_call
    return frappe.handler.handle()
  File "apps/frappe/frappe/handler.py", line 49, in handle
    data = execute_cmd(cmd)
  File "apps/frappe/frappe/handler.py", line 85, in execute_cmd
    return frappe.call(method, **frappe.form_dict)
  File "apps/frappe/frappe/__init__.py", line 1718, in call
    return fn(*args, **newargs)
  File "apps/frappe/frappe/utils/typing_validations.py", line 31, in wrapper
    return func(*args, **kwargs)
  File "apps/frappe/frappe/__init__.py", line 878, in wrapper_fn
    retval = fn(*args, **get_newargs(fn, kwargs))
  File "apps/frappe/frappe/desk/reportview.py", line 30, in get
    data = compress(controller.get_list(args))
  File "apps/frappe/frappe/core/doctype/rq_job/rq_job.py", line 88, in get_list
    jobs = [serialize_job(job) for job in Job.fetch_many(job_ids=matched_job_ids, connection=conn) if job]
  File "apps/frappe/frappe/core/doctype/rq_job/rq_job.py", line 88, in <listcomp>
    jobs = [serialize_job(job) for job in Job.fetch_many(job_ids=matched_job_ids, connection=conn) if job]
  File "apps/frappe/frappe/core/doctype/rq_job/rq_job.py", line 137, in serialize_job
    job_kwargs = job.kwargs.get("kwargs", {})
  File "env/lib/python3.10/site-packages/rq/job.py", line 553, in kwargs
    self._deserialize_data()
  File "env/lib/python3.10/site-packages/rq/job.py", line 488, in _deserialize_data
    raise DeserializationError() from e
rq.exceptions.DeserializationError


### Request Data

{
	"type": "POST",
	"args": {
		"doctype": "RQ Job",
		"fields": "[\"`tabRQ Job`.`name`\",\"`tabRQ Job`.`owner`\",\"`tabRQ Job`.`creation`\",\"`tabRQ Job`.`modified`\",\"`tabRQ Job`.`modified_by`\",\"`tabRQ Job`.`_user_tags`\",\"`tabRQ Job`.`_comments`\",\"`tabRQ Job`.`_assign`\",\"`tabRQ Job`.`_liked_by`\",\"`tabRQ Job`.`docstatus`\",\"`tabRQ Job`.`idx`\",\"`tabRQ Job`.`queue`\",\"`tabRQ Job`.`status`\",\"`tabRQ Job`.`job_name`\"]",
		"filters": "[]",
		"order_by": "`tabRQ Job`.`modified` desc",
		"start": 0,
		"page_length": 20,
		"view": "List",
		"group_by": null,
		"with_comment_count": 1
	},
	"freeze": false,
	"freeze_message": "Loading...",
	"headers": {},
	"error_handlers": {},
	"url": "/api/method/frappe.desk.reportview.get",
	"request_id": null
}

### Response Data

{
	"exception": "rq.exceptions.DeserializationError",
	"exc_type": "DeserializationError"
}
```

**Solution:**
If you are getting the error: "exception": `"rq.exceptions.DeserializationError"` Then try restarting mysql using the following command:

``` sudo service mysql restart ```

This should fix the issue in most cases. In case if it didn't work try the following as well:

``` sudo service nginx restart ```

``` sudo service supervisor restart ```

If nothing seems to work just do:

``` sudo reboot ```
