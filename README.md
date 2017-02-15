# kinesalite-nexe

Single binary for [Kinesalite](https://github.com/mhart/kinesalite).

## Build
```shell
# Setup
docker 
url --silent --location https://rpm.nodesource.com/setup_7.x | bash -
yum install -y git make gcc gcc-c++ make nodejs
npm install nexe -g

# Checkout
git clone https://github.com/mhart/kinesalite.git

# Patch (see https://github.com/nexe/nexe#doesnt-support-dynamic-require-statements)
git apply <<EOF
[root@2efa80d9707f sandbox]# cat patch
diff --git a/index.js b/index.js
index bf3715a..8159c91 100644
--- a/index.js
+++ b/index.js
@@ -49,11 +49,50 @@ function kinesalite(options) {
   return server
 }

-validOperations.forEach(function(action) {
-  action = validations.toLowerFirst(action)
-  actions[action] = require('./actions/' + action)
-  actionValidations[action] = require('./validations/' + action)
-})
+actions['addTagsToStream'] = require('./actions/addTagsToStream')
+actionValidations['addTagsToStream'] = require('./validations/addTagsToStream')
+
+actions['createStream'] = require('./actions/createStream')
+actionValidations['createStream'] = require('./validations/createStream')
+
+actions['deleteStream'] = require('./actions/deleteStream')
+actionValidations['deleteStream'] = require('./validations/deleteStream')
+
+actions['describeStream'] = require('./actions/describeStream')
+actionValidations['describeStream'] = require('./validations/describeStream')
+
+actions['getRecords'] = require('./actions/getRecords')
+actionValidations['getRecords'] = require('./validations/getRecords')
+
+actions['getShardIterator'] = require('./actions/getShardIterator')
+actionValidations['getShardIterator'] = require('./validations/getShardIterator')
+
+actions['listStreams'] = require('./actions/listStreams')
+actionValidations['listStreams'] = require('./validations/listStreams')
+
+actions['listTagsForStream'] = require('./actions/listTagsForStream')
+actionValidations['listTagsForStream'] = require('./validations/listTagsForStream')
+
+actions['mergeShards'] = require('./actions/mergeShards')
+actionValidations['mergeShards'] = require('./validations/mergeShards')
+
+actions['putRecord'] = require('./actions/putRecord')
+actionValidations['putRecord'] = require('./validations/putRecord')
+
+actions['putRecords'] = require('./actions/putRecords')
+actionValidations['putRecords'] = require('./validations/putRecords')
+
+actions['removeTagsFromStream'] = require('./actions/removeTagsFromStream')
+actionValidations['removeTagsFromStream'] = require('./validations/removeTagsFromStream')
+
+actions['splitShard'] = require('./actions/splitShard')
+actionValidations['splitShard'] = require('./validations/splitShard')
+
+actions['increaseStreamRetentionPeriod'] = require('./actions/increaseStreamRetentionPeriod')
+actionValidations['increaseStreamRetentionPeriod'] = require('./validations/increaseStreamRetentionPeriod')
+
+actions['decreaseStreamRetentionPeriod'] = require('./actions/decreaseStreamRetentionPeriod')
+actionValidations['decreaseStreamRetentionPeriod'] = require('./validations/decreaseStreamRetentionPeriod')

 function sendRaw(req, res, body, statusCode) {
   req.removeAllListeners()
@@ -246,3 +285,4 @@ function httpHandler(store, req, res) {

 if (require.main === module) kinesalite().listen(4567)

+
EOF


# Build
nexe -i cli.js -o ./kinesalite

git apply 
```

## Deploy

Copy to `dist/<version>/<tarball>` and commit
