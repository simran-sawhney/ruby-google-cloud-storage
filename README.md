# ruby-google-cloud-storage-steps
Steps to upload assets at google cloud storage, dynamic bucket handling, details of files uploaded, etc. See README for more details


# step 1 (Install the gem dependencies and add below lines in gemfile)
	gem 'google-cloud-storage'
	gem 'google-api-client', '~> 0.11.0'

# step 2 (create intializer file named 'google_cloud_storage.rb' or any other required name and add below lines)
	require "google/cloud/storage"
	GoogleCloudStorage = Google::Cloud::Storage.new(
	 project: "gd-test-167412",
	 keyfile: Rails.root.join('gd-test-8672a93d6171.json')
	)

	#in above: project value is the global project-id 
	#keyfile is json file generated from IAM panel in google console. (mentioned below) and add file name inside

# step 3 (create json key file from IAM google console.)
	1. Goto IAM in console
	2. create service account
	3. add some service account name in name field
	4. in roles, scroll down to storage and select as 'storage admin'
	5. check the 'Furnish a new private key' checkbox and select JSON
	6. click on create and save the downloaded json file in root folder of app and replace the name in google_cloud_storage.rb file accordingly

# step 4
	-- see the possible methods from http://googlecloudplatform.github.io/google-cloud-ruby/#/docs/google-cloud-storage/v1.6.0/google/cloud/storage

### Example to upload image at gcs using above configurations.

	 -- bucket = GoogleCloudStorage.bucket "my-todo-app" 
	 # GoogleCloudStorage is the variable defined in google_cloud_storage initializer file.
	 # "my-todo-app" is the name of the bucket already created at GCS. if bucket doesn't exist, create it using "bucket = storage.bucket "my-todo-app"

	 -- file = bucket.create_file 'C:\Users\Simran\Desktop\memw0pud152o.png', 'testimage.png'

	 -- possible return values from file
	 	> file.gapi.bucket (name of bucket)
		> file.gapi.size   (in bytes)
		> file.gapi.content_type ("images/png")
		> file.gapi.name  (name of file uploaded)
		> file.gapi.self_link (absolute URL of the asset)
		> file.gapi.updated   (Time.at(file.gapi.updated))


### Example for dynamically naming the bucket (let us say name of bucket we want is "dynamic-bucket")
	> bucket = GoogleCloudStorage.bucket "dynamic-bucket"
	> bucket = GoogleCloudStorage.create_bucket "dynamic-bucket" unless bucket.present? # this will create the required bucket if doesn't exists
	> ...


### Other Gems to do the same stuff
	1. FOG (reference URL)
		> https://github.com/fog/fog-google
		> https://cloud.google.com/ruby/getting-started/using-cloud-storage#uploading_to_cloud_storage
		> http://www.thagomizer.com/blog/2016/09/30/ruby-gcp-uploading-pictures-to-cloud-storage.html
		> http://www.thagomizer.com/blog/2016/10/28/using-fog-google.html

	2. paperclip-gcs (reference URL)
		> https://github.com/daichirata/paperclip-gcs
