# ownerinfo
I am trying to get the Owner information form hubspot
{% set user = request_contact.contact %}
{% set contact_id = user.hs_object_id %} 
{% set company_id = request_contact.company.hs_object_id %} 

<!-- Determine the appropriate search criteria based on the logged in Contact's "portal_role" value.  -->
{% set company_or_contact_id = user.portal_role == "Super Admin" ? company_id : contact_id %}
{% set association_type_id = user.portal_role == "Super Admin" ? 25 : 15 %}

{% set objects = crm_objects('deal', 'pipeline=4fc41d6d-9d36-4914-a704-5c82af15fb06&firstname__not_null=limit=5&order=-createdate', 'dealname, amount, closedate, pipeline, dealstage, hs_createdate, hs_lastmodifieddate,
  createdate, hubspot_owner_id, dealname, dealstage, hs_is_closed, hs_closed_amount, associations.company, partner_account, hs_projected_amount_in_home_currency, poc_company_name, firstname' ) %}

{% set ticket_array = objects.results %}

<table id="dealtable" class="table table-hover ticket-table">
    			<thead>
				<tr class="header-row">
          <th scope="col">DEAL NAME</th>
					<th scope="col">DEALS STAGE</th>
					<th scope="col">CLOSE DATE</th>
					<th scope="col">DEAL OWNER</th>
					<th scope="col">AMOUNT</th>   
				</tr>
			</thead>
        	<tbody>
      {% if ticket_array|length > 0 %} 
      {% for deals in ticket_array %}
      {% if deals.source_type != "CHAT" and deals.source_type != "Support Automation" and deals.source_type != "Internal"
      and deals.source_type != "Community" and deals.source_type != "PS" and deals.source_type != "Chatter"%}
				<tr scope="row">
          <td data-label="DEAL NAME" class="gray-cell">
						 <a href="https://app.hubspot.com/contacts/7475627/deal/{{deals.id}}" target="_blank">{{deals.dealname}}</a>
          </td>  
        {% if deals.dealstage == "1a4e48ef-e214-468c-9d51-fe5547933286"%}
					<td data-label="DEAL STAGE">
           <span class="badge badge-pill p1">Prospecting</span>
          </td>
         {% elif deals.dealstage == "72c0e000-d43b-4ed9-a762-fd59723134bc"%}
					<td data-label="DEAL STAGE">
           <span class="badge badge-pill p2">Qualifying</span>
          </td>
         {% elif deals.dealstage == "79795644-3df1-4060-afbe-560f26a8aaad"%}
					<td data-label="DEAL STAGE">
           <span class="badge badge-pill p3">Developing Solution</span>
          </td>
         {% elif deals.dealstage == "3e872ef2-3101-4dbf-a85e-417396969466"%}
					<td data-label="DEAL STAGE">
           <span class="badge badge-pill p4">Negotiating</span>
          </td>
         {% elif deals.dealstage == "0a14ba1e-dacd-4ce5-8ddf-ead56613e16b"%}
					<td data-label="DEAL STAGE">
           <span class="badge badge-pill p5">Won</span>
          </td>
         {% else %}
          <td data-label="DEAL STAGE">
            {{deals.dealstage}}
          </td>
         {% endif %}         
         <td data-label="CLOSE DATE" class="gray-cell">{{deals.closedate}}</td>  
         <td id="idProperty" data-label="DEAL OWNER"class="gray-cell">{{deals.closedate}}</td>
         <td data-label="AMOUNT" class="gray-cell">{{deals.amount}}</td> 
				</tr>
    {% endif %}
    {% endfor %}
    {% endif %}
    </tbody>
  </table>  

