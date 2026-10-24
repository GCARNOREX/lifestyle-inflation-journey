# Deploy to AWS S3 and CloudFront

1. Create a private S3 bucket and keep **Block Public Access** enabled.
2. Create a CloudFront distribution with the bucket as its origin. Configure Origin Access Control (OAC) and set the default root object to `index.html`.
3. Configure AWS credentials in your local AWS CLI, outside this project folder. For a named access-key profile, run `aws configure --profile lifestyle-game` and enter the Access Key ID, Secret Access Key, region, and output format when prompted. The CLI stores credentials in your home directory's `.aws/credentials` file; never put them in `index.html`, `game.js`, or any file you upload. If your account uses IAM Identity Center, use `aws configure sso --profile lifestyle-game` instead.
4. From this project folder, upload the site files using that profile:

   ```sh
   aws s3 sync . s3://BUCKET --exclude "*.md" --profile lifestyle-game
   ```

5. After changes, invalidate the CloudFront cache:

   ```sh
   aws cloudfront create-invalidation --distribution-id DISTRIBUTION_ID --paths "/*" --profile lifestyle-game
   ```

The site is plain HTML, CSS, JavaScript, and relative SVG assets; it has no build step or backend.
