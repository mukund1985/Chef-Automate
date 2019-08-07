# Generating Custom Reports from Chef Automate 2.0 compliance API
The following documents how to query the A2 compliance api to pull reports for a given node for a given timeframe

## step one: use suggestions or nodes list to get the id of the node
this will return a list of suggestions.
they will want to search the response data for the node name they requested and then grab the id for the node
        curl -H "api-token: admin-token-val" https://a2-url/api/v0/compliance/reporting/suggestions -d '{"type":"node","text":"angrychef-rhel-7-s390x-tester-76dc19.cd.chef.co","filters":[{"type":"start_time","values":["2019-04-02T00:00:00Z"]},{"type":"end_time","values":["2019-04-12T23:59:59Z"]}]}'
as an alternative to the above api call, the user could call nodes list and search for the name in the results, grabbing the id from the object there

      curl -H "api-token: admin-token-val" https://a2-url/api/v0/compliance/reporting/nodes/search -d '{"filters":[{"type":"start_time","values":["2019-04-02T00:00:00Z"]},{"type":"end_time","values":["2019-04-12T23:59:59Z"]}],"page":1,"per_page":100,"sort":"latest_report.end_time","order":"DESC"}'
step two: use the node id to retrieve a list of all reports for that node given time frame
use the node id to get a list of all reports given a time frame

    curl -H "api-token: admin-token-val" https://a2-url/api/v0/compliance/reporting/reports -d '{"filters":[{"type":"start_time","values":["2019-04-02T00:00:00Z"]},{"type":"end_time","values":["2019-04-12T23:59:59Z"]},{"type":"node_id","values":["ea3581e9-4f00-43ec-acbf-1f76a96ae343"]}],"sort":"latest_report.end_time","order":"DESC"}'
step three: iterate through the list of report ids and retrieve each one
iterate through the list of reports returned from above api call to pull each one

    curl -H "api-token: admin-token-val" https://a2-url/api/v0/compliance/reporting/reports/id/b07549a6-b49d-4c16-9ce3-996fa
