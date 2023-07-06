# Check Subgraph Health

Background: There have been periods where the subgraphs that feed data for the perpetuals platforms have gone down. When this happens, the analytics page goes down and chart data is not updated on the perpetuals trading page. To overcome this issue, we need an alert bot that will send an SMS message if the subgraphs, or other OmniDex APIs go down.


Here is a list of the following Subgraphs that need to be periodically queried to ensure they are still synced.


https://data.subgraph.omnidex.finance/subgraphs/name/perpetuals-stats

https://data.subgraph.omnidex.finance/subgraphs/name/prices

https://subgraph.omnidex.finance/subgraphs/name/perpetuals-referrals

https://subgraph.omnidex.finance/subgraphs/name/perpetuals-raw


For the subgraphs, you will need to query the latest block they have synced to and check it against the latest block number on tEVM. If they are out sync by over 100 blocks, send an SMS message.

 The data structure of the query is as follows:

    query MyQuery {
    	  _meta {
    		    block {
    			      number
    			      }
    		 }
    }




Hint: Here is some python code that queries data from our referrals subgraph:

    batch_positions  =  1000
    
    position_counter  =  0
    
    referrerStats  =  f'''
    
    query {{
    
    referralStats(first: {batch_positions} skip: {position_counter} where: {{period: total}}) {{
    
    id
    
    discountUsdCumulative
    
    }}
    
    }}
    
    '''
    
    headers  = {'Content-Type': 'application/json'}
    
    body  =  json.dumps({'query': referrerStats})
    
    response  =  requests.post(SUBGRAPH_URL, headers=headers, data=body)
    
    data  =  response.json()["data"]["referralStats"]
    

**Part 2**
Write a script that checks when the function "setPriceWithBits" and 
"setPricesWithBitsAndExecute"  were last updated for the contract. "0xAFE63F26E69e41374a96A9a0bB109BfF9Fce782d".
If neither functions have been called for more than 10mins, send an SMS alert. 



