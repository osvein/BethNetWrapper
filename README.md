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

exe dump:
	305f088 /ugc-content/remove
	305f0a0 /ugc-content/list/me
	305f0b8 /ugc-content/list/subscribe
	305f0d8 /ugc-content/add-subscription
	305f0f8 /ugc-content/unsubscribe
	305f118 /ugc-content/list/followed
	305f138 /ugc-content/follow
	305f150 /ugc-content/unfollow
	305f168 /ugc-content/author-follow
	305f1a8 /ugc-content/author-unfollow
	305f1c8 /ugc-workshop/list/featured-mods
	305f1f0 /ugc-workshop/list/featured-cc-mods
	305f218 /ugc-workshop/list
	3062bf8 /log/collect_errordata
	30641d0 /ugc-content/increment-downloads
	3064c20 /ugc-workshop/list/categories
	3065458 /ugc-content/upload
	30654f0 /ugc-content/upload-image
	30656b8 /ugc-content/media/add-video
	3065a18 /ugc-upload/details
	3065cb0 /ugc-content/comment
	3065dc8 /ugc-content/create
	30662f0 /ugc-content/edit
	3066458 /ugc-content/flag
	3066470 /ugc-comment/flag
	3066ad8 /ugc-static/list/categories
	3066eb8 /ugc-static/list/dlc/
	3067390 /ugc-static/list/platforms
	3067770 /ugc-static/list/products
	3067c90 /ugc-content/list/followed-authors
	3068228 /ugc-workshop/blacklist
	3068980 /ugc-notification/ack
	3068ae8 /ugc-content/upload-preview
	3068be8 /ugc-content/rate
	3068ce8 /ugc-comment/like
	3068de8 /ugc-content/add-release-note
	3069080 /ugc-content/update-release-note
	30691a0 /ugc-content/remove-release-note
	30692a8 /ugc-content/refresh-entitlement
	306b7a0 /agora_beam/legal_documents/accept
	306c2a8 /agora_beam/accounts/link
	306cd98 /agora_beam/accounts
	306d1e0 /agora_beam/accounts/linkable
	306e180 /agora_beam/accounts/check
	306e398 /agora_beam/accounts/fingerprints
	306e500 /agora_beam/accounts/me
	306e670 /agora_beam/accounts/check_email
	306e800 /agora_beam/accounts/recover/password
	306e828 /agora_beam/accounts/recover/username
	306ea38 /agora_beam/accounts/resend_verification
	306f648 /entitlements/products/
	306f680 /entitlements/entitlement-mappings
	3074980 /wallet/balance
	3074c80 /mtx/purchase
	3074f60 /fulfillment/update_first_party_entitlements
	3075110 /cms/message
	3075ab8 /status/ext-server-status
	3075c30 /ugc-upload/cancel
	3075d38 /ugc-upload/complete
	3075e48 /ugc-upload/initiate
	30760a8 /ugc-upload/part
	3076820 /cdp-user/projects/
	3076838 /branches/
	307d6a8 /agora_beam/legal_documents/required
	307e568 /agora_beam/accounts/create_quick
	307e894 /login
	307eb08 /agora_beam/accounts/login
	307ed18 /agora_beam/accounts/external_login
	307ee30 /external-login
	307ef28 /agora_beam/accounts/upgrade_token
	307f558 /agora_beam/accounts/retrieve_external_account
	3088808 /refresh-session
	3089528 /agora_hydra/access
	3089888 /agora_hydra/auth
	3089b00 /cdp-user/auth
	308e180 /logout
