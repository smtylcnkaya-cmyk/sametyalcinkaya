# sametyalcinkaya
Geleceği İnşa Eden Çözümler
mongosh --eval \"
use('test_database');
var userId = 'test-user-' + Date.now();
var sessionToken = 'test_session_' + Date.now();
db.users.insertOne({
  user_id: userId,
  email: 'test.user.' + Date.now() + '@example.com',
  name: 'Test User',
  picture: 'https://via.placeholder.com/150',
  created_at: new Date()
});
db.user_sessions.insertOne({
  user_id: userId,
  session_token: sessionToken,
  expires_at: new Date(Date.now() + 7*24*60*60*1000),
  created_at: new Date()
});
print('Session token: ' + sessionToken);
print('User ID: ' + userId);
\"
```

## Step 2: Test Backend API
```bash
# Test auth endpoint
curl -X GET \"https://your-app.com/api/auth/me\" \
  -H \"Authorization: Bearer YOUR_SESSION_TOKEN\"

# Test protected endpoints
curl -X GET \"https://your-app.com/api/projects\" \
  -H \"Authorization: Bearer YOUR_SESSION_TOKEN\"
```

## Step 3: Browser Testing
```javascript
// Set cookie and navigate
await page.context.add_cookies([{
    \"name\": \"session_token\",
    \"value\": \"YOUR_SESSION_TOKEN\",
    \"domain\": \"your-app.com\",
    \"path\": \"/\",
    \"httpOnly\": true,
    \"secure\": true,
    \"sameSite\": \"None\"
}]);
await page.goto(\"https://your-app.com\");
```

## Quick Debug
```bash
# Check data format
mongosh --eval \"
use('test_database');
db.users.find().limit(2).pretty();
db.user_sessions.find().limit(2).pretty();
\"

# Clean test data
mongosh --eval \"
use('test_database');
db.users.deleteMany({email: /test\.user\./});
db.user_sessions.deleteMany({session_token: /test_session/});
\"
```

## Checklist
- [ ] User document has user_id field
- [ ] Session user_id matches user's user_id exactly
- [ ] All queries use `{\"_id\": 0}` projection
- [ ] Backend queries use user_id (not _id or id)
- [ ] API returns user data with user_id field
- [ ] Browser loads dashboard (not login page)

## Success Indicators
✅ /api/auth/me returns user data
✅ Dashboard loads without redirect
✅ CRUD operations work

## Failure Indicators
❌ \"User not found\" errors
❌ 401 Unauthorized responses
❌ Redirect to login page
"
Observation: Create successful: /app/auth_testing.md

Fetching debug logs for expo

==> /var/log/supervisor/expo.err.log <==

==> /var/log/supervisor/expo.out.log <==
Starting project at /app/frontend
Fast resolver is enabled.
Metro is running in CI mode, reloads are disabled. Remove CI=true to enable watch mode.
Starting Metro Bundler
Tunnel connected.
Tunnel ready.
Waiting on http://localhost:3000
Logs for your project will appear below.
yarn run v1.22.22
$ /app/frontend/node_modules/.bin/expo start --tunnel --port 3000
env: load .env
env: export EXPO_TUNNEL_SUBDOMAIN EXPO_PACKAGER_HOSTNAME EXPO_PUBLIC_BACKEND_URL EXPO_USE_FAST_RESOLVER METRO_CACHE_ROOT
Starting project at /app/frontend
Fast resolver is enabled.
Metro is running in CI mode, reloads are disabled. Remove CI=true to enable watch mode.
Starting Metro Bundler
Tunnel connected.
Tunnel ready.
Waiting on http://localhost:3000
Logs for your project will appear below.


Fetching debug logs for backend

==> /var/log/supervisor/backend.err.log <==
INFO:     Will watch for changes in these directories: ['/app/backend']
INFO:     Uvicorn running on http://0.0.0.0:8001 (Press CTRL+C to quit)
INFO:     Started reloader process [92] using WatchFiles
INFO:     Started server process [126]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Shutting down
INFO:     Waiting for application shutdown.
INFO:     Application shutdown complete.
INFO:     Finished server process [126]
INFO:     Stopping reloader process [92]
INFO:     Will watch for changes in these directories: ['/app/backend']
INFO:     Uvicorn running on http://0.0.0.0:8001 (Press CTRL+C to quit)
INFO:     Started reloader process [227] using WatchFiles
INFO:     Started server process [271]
INFO:     Waiting for application startup.
INFO:     Application startup complete.

==> /var/log/supervisor/backend.out.log <==


Fetching debug logs for mongodb
