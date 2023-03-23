web_passman
# Web: Passman
> Pandora discovered the presence of a mole within the ministry. To proceed with caution, she must obtain the master control password for the ministry, which is stored in a password manager. Can you hack into the password manager?


#### Difficulty: easy

We are given a zip file that contains several files, notably: the `challenge` directory which contains the source code, `Dockerfile`, `build-docker.sh` and `entrypoint.sh`.

Running `npm audit` did not give any immediate high severity CVEs, only a moderate one on jsonwebtoken. After reviewing the database and JWT implementation, I concluded that neither are vulnerable.

Looking through the `challenge` folder, I spotted `GraphqlHelper.js`. After some googling, I found out that `graphiql` can be set to true to get a web GUI for graphql. I set this to true in `index.js` as I am inexperienced with GraphQL. After building and running the image using `./build-docker.sh`, I was able to access the graphiql interface via `http://localhost:1337/graphql`.

After studying the `GraphqlHelper.js` file, I saw that the `UpdatePassword` mutation will let me update using just a username, the desired password, and any token. I created a new user and used the token from that user to authorize the mutation that I sent via curl:

```
curl 'http://165.232.98.59:32321/graphql' \
  -X POST \
  -b 'session=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3QiLCJpc19hZG1pbiI6MCwiaWF0IjoxNjc5NDU5MDA2fQ.05Wu_XqtvAlyGhGtZRMF9uLDAzSgt1bXuvFPjq56Pig' \
  -H 'content-type: application/json' \
  --data '{
    "query":"mutation { UpdatePassword(username: \"admin\", password: \"admin\") {message token}}"
  }'
 ```

After executing the command above, I logged in as admin, which gave the phrases for the admin user, which gave me the flag:

`HTB{1d0r5_4r3_s1mpl3_4nd_1mp4ctful!!}`
