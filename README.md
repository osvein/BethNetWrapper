# BethNetWrapper
C wrapper for the Bethesda.net API

Servers/endpoints/resources:

	account.bethesda.net:443
	GET /status HTTP/1.1
	GET /en/api/signout HTTP/1.1
		clears all the cookies except bnet-remember-username
	POST /en/login HTTP/1.1
		fp
		username
		password //plaintext
		rememberUsername=on
		sets bnet-username, bnet-session and bnet-remember-username
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
		content_id
	GET /ugc-workshop/list/author HTTP/2.0
		author_id
	GET /ugc-content/list/subscribe HTTP/2.0
		number_results
		order
		page
		platform
		product
		sort
		text
	GET /ugc-content/list/me HTTP/2.0
		number_results
		order
		page
		platform
		product
		sort
		text
		broken=true
	GET /ugc-content/list/followed HTTP/2.0
		number_results
		order
		page
		platform
		product
		sort
		text
	OPTIONS /ugc-content/add-subscription
	POST /ugc-content/add-subscription
		content_id
	OPTIONS /ugc-content/unsubscribe
	DELETE /ugc-content/unsubscribe
		content_id
	OPTIONS /ugc-content/unfollow HTTP/2.0
	DELETE /ugc-content/unfollow HTTP/2.0
		content_id
	OPTIONS /ugc-content/follow HTTP/2.0
	POST /ugc-content/follow HTTP/2.0
		content_id
	GET /ugc-content/moderation-categories HTTP/2.0
		product
	GET /bwa/auth HTTP/2.0
		code
		fp
		state
		in goes bnet-username cookie, out comes bnet-workshop
	Accept: application/json

	bethesda.net:443
	GET /community/comments/get/mods/mods_%d/%d HTTP/2.0
		1: id
		2: page
		_=1507127655114
	GET /community/comments/isLoggedIn HTTP/2.0
		_=1507144755294
		buid //looks like some sort of uuid
	GET /communityapi/ssobethesda/logout HTTP/2.0
		
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

	query param "product"
		fallout4
		skyrim
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
	cookie "bnet-username"
		JSON object
			username
			lang
			buid
			country
	cookie "bnet-workshop"		
	cookie "bnet-remember-username"
		JSON object
			username
	cookie "bnet-redirect"
	cookie "bnet-session"
	cookie "bnet-join-user"
	cookie "bnet-message"
	cookie "bnet-oauth-params"
	cookie "bnet-workshop"

Bethesda Community is based on NodeBB
