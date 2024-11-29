customEvents  
| where customDimensions  contains "521d0882-f196-4c16-bb29-99b5beefc985"


customEvents  
| where customDimensions  contains "521d0882-f196-4c16-bb29-99b5beefc985"

customEvents| where operation_ParentId == '92677a2ec8f42ede' | order by timestamp

customEvents  
| where name == 'NegativeMessageResponse'
| where customDimensions  contains "PuOHeadId ee81b08f-4cc0-4801-9fda-f33538ef5960"