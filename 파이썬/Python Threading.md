
```python
import threading
import time

# Simulate publishing a blog post
def publish_post(post_id):
    print(f"Starting to publish post {post_id}...")
    time.sleep(3)  # Simulate time taken to publish a post
    print(f"Post {post_id} published successfully!")

# Simulate moderating comments
def moderate_comments(post_id):
    print(f"Moderating comments for post {post_id}...")
    time.sleep(2)  # Simulate time taken for moderation
    print(f"Comments for post {post_id} moderated!")

# Simulate sending notifications
def send_notifications(post_id):
    print(f"Sending notifications for post {post_id}...")
    time.sleep(1)  # Simulate time taken to send notifications
    print(f"Notifications for post {post_id} sent!")

# Main function to simulate blogging workflow
def blogging_workflow(post_id):
    # Create threads for each task
    publish_thread = threading.Thread(target=publish_post, args=(post_id,))
    moderate_thread = threading.Thread(target=moderate_comments, args=(post_id,))
    notify_thread = threading.Thread(target=send_notifications, args=(post_id,))

    # Start the threads
    publish_thread.start()
    moderate_thread.start()
    notify_thread.start()

    # Wait for all threads to finish
    publish_thread.join()
    moderate_thread.join()
    notify_thread.join()

    print(f"Blogging workflow for post {post_id} completed.")

# Simulate multiple posts being published
if __name__ == "__main__":
    post_ids = [1, 2, 3]  # Example post IDs
    for post_id in post_ids:
        print(f"\nStarting workflow for post {post_id}...")
        blogging_workflow(post_id)
```

### Explanation

1. **Threads for Parallel Tasks**:  
   Each task (publishing, moderating, sending notifications) is handled by a separate thread, allowing them to execute concurrently.

2. **Simulating Time-Consuming Tasks**:  
   `time.sleep` is used to mimic tasks that take time to complete.

3. **Workflow**:  
   The `blogging_workflow` function creates threads for each task, starts them, and waits for all threads to finish using `join`.

4. **Scalability**:  
   The script can handle multiple posts by iterating over a list of post IDs and running the workflow for each.

### Sample Output
```
Starting workflow for post 1...
Starting to publish post 1...
Moderating comments for post 1...
Sending notifications for post 1...
Notifications for post 1 sent!
Comments for post 1 moderated!
Post 1 published successfully!
Blogging workflow for post 1 completed.

Starting workflow for post 2...
...
``` 

This approach demonstrates how threading can make your blogging system handle tasks efficiently in parallel.
