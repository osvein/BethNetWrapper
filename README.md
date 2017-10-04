# BethNetWrapper
C wrapper for the Bethesda.net API

Servers/endpoints/resources:

	account.bethesda.net:443
	GET /status HTTP/1.1
	Accept: application/json

	services.bethesda.net:443
	GET / HTTP/2.0
	Accept: application/json
	
	mods.services.bethesda.net:443
	GET / HTTP/2.0
	GET /ugc-static/list/platforms HTTP/2.0
	GET /ugc-workshop HTTP/2.0
	GET /ugc-workshop/list HTTP/2.0
		product=fallout4
		number_results=0
		content_ids=[947931,3110042,4020018]
	GET /ugc-admin/list/moderation HTTP/2.0
		number_results=0
		product=fallout4
	GET /ugc-workshop/list/categories HTTP/2.0
		platform //see below
		product=fallout4
		text //search
		number_results=20
		page=1
		sort //see below
		category=Animals
	GET /ugc-workshop/list/ HTTP/2.0
		platform //see below
		number_results=20
		order=desc
		page=1
		product=fallout4
		sort  //see below
		text=
		category=["Animals"]
	GET /ugc-workshop/content/get HTTP/2.0
		content_id //content id
	GET /ugc-workshop/list/author HTTP/2.0
		author_id //author id
	Accept: application/json

	bethesda.net:443
	GET /community/comments/get/mods/mods_%d/%d HTTP/2.0
		1: id
		2: page
		_=1507127655114
	Accept: application/json
	
Response format:

	{
		"platform": {
			"message": "%s",
			"code": %f,
			"response": {
				...
			}
		}
	}

Observations:

	query param "sort" for
	/ugc-workshop/list/categories
	/ugc-workshop/list/
		%s%s
		1:
			popular <- Most Popular
			published <- Latest
			rating <- Highest Rated
			follow <- Most Favorited
		2:
			 <- All Time
			-day <- Daily
			-week <- Last Week
			-month <- Last Month
	query param "platform"
		%s
		1:
			 <- All Platforms
			WINDOWS <- PC
			XB1 <- Xbox One
			PS4 <- PlayStation 4
	query param "category"
		array of category names
	for
	/ugc-workshop/list/categories
		GET array
	/ugc-workshop/list/
		JSON array
