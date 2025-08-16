# AWS S3 → SNS Email Notifications

A small, event-driven setup: when a file is **uploaded to Amazon S3**, an **SNS topic** sends an **email** to subscribed users. This shows cloud-native integration with **no custom compute**.

## What & Why
- **What:** Automatic email alert on every new object in an S3 bucket.
- **Why:** Decoupled, serverless notifications for audit trails, data ingestion, or team alerts.

## How It Works
1. **S3** emits an *ObjectCreated* event when a file is uploaded.
2. The bucket’s **Event notifications** forward that event to an **SNS topic**.
3. **SNS** fans-out and delivers an **email** to each confirmed subscription.
4. An **SNS topic policy** allows `s3.amazonaws.com` to publish **only** from your bucket’s ARN.

## Reproduce (AWS Console)
1. Create **SNS topic** → Standard → name it (e.g., `S3EventNotifications`).
2. Create **Email subscription** → your address → **Confirm** from your inbox.
3. Create **S3 bucket** → unique name in the same region as the SNS topic.
4. In the bucket: **Properties → Event notifications → Create**  
   - Event types: **All object create events**  
   - Destination: **SNS topic** → pick your topic
5. **Test**: upload any file to the bucket → you should receive an email.

## Screenshots (Outcome)
**1. Add Files in S3**  
![Add Files](docs/1.png)  
*Click the created S3 bucket and choose **Add files** to start an upload.*

**2. Upload Confirmation**  
![Upload](docs/2.png)  
*Select the files and click **Upload** to create the objects (triggers ObjectCreated).*

**3. Email Notification**  
![Email](docs/3.png)  
*Amazon SNS sends a message to the subscribed email address.*

## Repo Structure
