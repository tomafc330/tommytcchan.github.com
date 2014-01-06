---
layout: post
title: "Groovy: The way to populate a csv/excel file."
date: 2013-03-08 13:52
comments: true
categories: [Groovy, currying]
---
I was working on a refactoring for a method that did csv export, and I found a prime a candidate for using the currying mechanism in Groovy.

Consider this example of the code before:

```
	def export = {
		final string[] csvcolumnheadings = ['game', 'campaign name', 'campaign medium', 'campaign code', 'campaign term', 'campaign content',
			'campaign source', 'additional paramter','status', 'created by', 'updated by', 'date created', 'last updated']
		
		...

			campaigns.resultset.each { campaign ->
				string[] row = new string[13]
				
				row[0] = campaign.game ? campaign.game : ''
				row[1] = campaign.campaignname ? campaign.campaignname : ''
				row[2] = campaign.type ? campaign.type.tostring() : ''
				row[3] = campaign.campaigncode ? campaign.campaigncode : ''
				row[4] = campaign.campaignterm ?: ''
				row[5] = campaign.campaigncontent?: ''
				row[6] = campaign.source ? campaign.source : ''
				row[7] = campaign.parameter ? campaign.parameter : ''
				row[8] = campaign.status.name ? campaign.status.name : ''
				row[9] = campaign.createdby ? campaign.createdby : ''
				row[10] = campaign.updatedby ? campaign.updatedby : ''
				row[11] = campaign.datecreated ? campaign.datecreated.getdatetimestring() : ''
				row[12] = campaign.lastupdated ? campaign.lastupdated.getdatetimestring() : ''
				
				csvwriter.writenext(row)
			}

		...
			
	}	
```

So what's the problem here? Well, I was adding a couple of fields to the export, and the problem is that there are 4 things I needed to do: 
* Add it to the ```csvcolumnheadings```
* Update the number of elements in decl of the array ```row```
* Add the elements
* Update all the numberings

Also, it's not a very Groovy way of doing it. Come curryin to the rescue!

```
	def valRetrieveFunc = {fieldName, campaign ->
			def result

			if (fieldName instanceof Closure) {
				result = fieldName(campaign)
			} else {
				result = campaign."${fieldName}"
			}

			return result ?: ''
		}

		final Map map = [
				'Game': valRetrieveFunc.curry('game'),
				'Campaign Name': valRetrieveFunc.curry('campaignName'),
				'Campaign Medium': valRetrieveFunc.curry({campaign -> return campaign.type?.toString()}),
				'Campaign Code': valRetrieveFunc.curry('campaignCode'),
				'Campaign Term': valRetrieveFunc.curry('campaignTerm'),
				'Campaign Content': valRetrieveFunc.curry('campaignContent'),
				'Campaign Source': valRetrieveFunc.curry('source'),
				'Additional Paramter': valRetrieveFunc.curry('parameter'),
				'Status': valRetrieveFunc.curry({campaign -> return campaign.status?.name}),
				'Created By': valRetrieveFunc.curry('createdBy'),
				'Updated By': valRetrieveFunc.curry('updatedBy'),
				'Date Created': valRetrieveFunc.curry({campaign -> return campaign.dateCreated?.getDateTimeString()}),
				'Last Updated': valRetrieveFunc.curry({campaign -> return campaign.lastUpdated?.getDateTimeString()})]

		final String[] csvColumnHeadings = map.keySet()

		...

			campaigns.resultSet.each { campaign ->
				def row = []
				csvColumnHeadings.each({
					row << map[it](campaign)
				})

				csvWriter.writeNext(row.toArray() as String[])
			}

		...					
```

Let me annotate this a little bit. The first thing we define was a func on how to get the parameter from the domain object. Notice for complex retrievals we can pass in a closure to how to fetch the object graph. The more important bit here is the ```curry``` method, which defines a new closure with the same content, except that the first parameter has been `curried` and embedded in the new closure. By using ```curry``` in this way, the caller of the curried closure doesn't need worry about the first parameter..they can just invoke it with the campaign object in this case (ie. ```row << map[it](campaign)```.