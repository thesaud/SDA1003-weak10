# SDA-1003-Saud-Alqurashi-
# Week-10 Assignment - MERN Stack Blog App Deployment on AWS

## Project Overview

This is a MERN stack blog application that has been deployed on AWS using free-tier eligible services. The application consists of:

- **Backend**: Express.js running on an EC2 instance.
- **Frontend**: React.js hosted via S3 static website hosting.
- **Database**: MongoDB Atlas is used to store blog data.
- **Media Uploads**: Handled via another S3 bucket with appropriate IAM permissions.

## Deployment Steps

### 1. **MongoDB Atlas Configuration**
   - Create a MongoDB Atlas account and set up a free cluster.
   - Allow EC2 IP in network access settings.
   - Create a database user and obtain the connection string.
   - Replace the MongoDB URI in the `.env` file with the connection string from MongoDB Atlas.

### 2. **S3 Bucket for Frontend**
   - Create a bucket in the `eu-north-1` region named `sda1003-blogapp-frontend`.
   - Disable "Block all public access" and enable static website hosting.
   - Add a bucket policy to allow public access:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::sda1003-blogapp-frontend/*"
         }
       ]
     }
     ```

### 3. **S3 Bucket for Media Uploads**
   - Create a second bucket named `sda1003-blogapp-media`.
   - Disable "Block all public access".
   - Configure CORS for browser upload support:
     ```json
     [
       {
         "AllowedHeaders": ["*"],
         "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
         "AllowedOrigins": ["*"],
         "ExposeHeaders": ["ETag"]
       }
     ]
     ```

### 4. **IAM User and Policy for S3 Media Bucket Access**
   - Create an IAM user `blog-app-user` with the necessary policies to allow access to S3 buckets.
   - Attach a custom policy to the user that allows actions like `s3:PutObject`, `s3:GetObject`, etc.

### 5. **EC2 Backend Setup**
   - Launch an EC2 instance (`t3.micro`) in the `eu-north-1` region using Ubuntu 22.04 LTS.
   - SSH into the EC2 instance and install necessary dependencies.
   - Clone the repository and configure the `.env` file with the necessary environment variables (e.g., MongoDB connection string, AWS S3 credentials).
   - Start the backend server using `pm2`.

### 6. **Frontend Setup**
   - Build the React app using `pnpm run build`.
   - Deploy the frontend to S3 using the AWS CLI:
     ```bash
     aws s3 sync dist/ s3://sda1003-blogapp-frontend/
     ```

### 7. **Testing the Deployment**
   - **Backend**: Ensure that the backend is running properly via PM2.
   - **Frontend**: Test the frontend by accessing the S3 URL: `https://sda1003-blogapp-frontend.s3.eu-north-1.amazonaws.com/index.html`.

## Project Setup

### Backend
1. Navigate to the backend directory:
   ```bash
   cd backend
npm install
pm2 start index.js --name "blog-backend"


## Frontend
Navigate to the frontend directory:

bash
Copy
cd frontend
Install dependencies:

bash
Copy
pnpm install
Build the frontend:

bash
Copy
pnpm run build
Deploy the frontend to S3:

bash
Copy
aws s3 sync dist/ s3://sda1003-blogapp-frontend/
