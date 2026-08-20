---
schema_version: "1.0.0"
document_id: "2e2e1f45a100a0efcfbbd4ff4124fa6a524071cda5b9cf69cf6f382ee5f1aaee"
company_key: "s-p-global-inc-common-stock"
company: "S&P Global Inc."
source_id: "s-p-global-inc-common-stock-rss-ff630ac34bbe"
canonical_url: "https://engineering.global.com/basics-of-python-multithreading-a21050189565"
published_at: "2024-04-15T12:41:30+00:00"
first_seen_at: "2026-07-20T04:36:47.908335+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:1c29a5bfaa9a63d260f85974569a84826a1ca3b4463b1ed2065f87438952458a"
---

# Basics of Python Multithreading

# **Basics of Python Multithreading**


[Global Engineering](https://global-engineering.medium.com/?source=post_page---byline--a21050189565---------------------------------------)


4 min read


·


Apr 15, 2024


--


*By Cherilynn Atkinson*


When a Python program is running, a thread executes instructions in the code. By default, one thread executes for a Python process. This thread is usually called the *main thread* .


The following code defines three functions. Each function sends a GET request to a specified webpage, waits for the server’s response to the HTTP request, then prints the status code of the response. The thread name and associated thread ID are also logged in the terminal. Here, the functions are executed sequentially — one after the other.


```text
import time   from loguru import logger   import threading   import requests    # Define functions which try to request a webpage and print the status code   def task1():       logger.info("Task 1 started")       logger.info(f"Task 1 thread: {threading.current_thread().__class__.__name__}; thread ID: {threading.current_thread().ident}")       response = requests.get("https://www.globalplayer.com")       logger.info(f"Task 1 status code: {response.status_code}")       logger.info("Task 1 completed")    def task2():       logger.info("Task 2 started")       logger.info(f"Task 2 thread: {threading.current_thread().__class__.__name__}; thread ID: {threading.current_thread().ident}")       response = requests.get("https://www.heart.co.uk")       logger.info(f"Task 2 status code: {response.status_code}")       logger.info("Task 2 completed")    def task3():       logger.info("Task 3 started")       logger.info(f"Task 3 thread: {threading.current_thread().__class__.__name__}; thread ID: {threading.current_thread().ident}")       response = requests.get("https://www.lbc.co.uk")       logger.info(f"Task 3 status code: {response.status_code}")       logger.info("Task 3 completed")    # Execute tasks sequentially   if __name__ == "__main__":       print("Sequential task execution:")       start_time = time.time()        task1()       task2()       task3()        end_time = time.time()       total_time = end_time - start_time       logger.info(f"Total time taken: {total_time} seconds")
```


The functions execute in sequence, so task1() needs to be completed before task2() can start. Task1() makes a GET request to[https://www.globalplayer.com](https://www.globalplayer.com/) , waits for the response and prints the status code of the response. Task3() starts when task2() is complete. It takes around 0.4s in total for task1(), task2() and task3() to run:


Press enter or click to view image in full size


For each task, the same thread is used — the main thread.


## Get Global Engineering’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


If the tasks could start independently, without waiting for other tasks to finish, the program could execute more quickly. To do this, we could introduce more threads, which will allow separate parts of the program to run out of sequence.


One method of running multiple threads simultaneously is by using ThreadPoolExecutor, from the concurrent.futures module in the Python Standard Library. This uses a pool of threads to execute the functions asynchronously.


```text
import time  from loguru import logger  from concurrent.futures import ThreadPoolExecutor  import requests  import threading   # Define functions which try to request a webpage and print the status code  def task1():      logger.info("Task 1 started")      logger.info(f"Task 1 thread: {threading.current_thread().__class__.__name__}; thread ID: {threading.current_thread().ident}")      response = requests.get("https://www.globalplayer.com")      logger.info(f"Task 1 status code: {response.status_code}")      logger.info("Task 1 completed")   def task2():      logger.info("Task 2 started")      logger.info(f"Task 2 thread: {threading.current_thread().__class__.__name__}; thread ID: {threading.current_thread().ident}")      response = requests.get("https://www.heart.co.uk")      logger.info(f"Task 2 status code: {response.status_code}")      logger.info("Task 2 completed")   def task3():      logger.info("Task 3 started")      logger.info(f"Task 3 thread: {threading.current_thread().__class__.__name__}; thread ID: {threading.current_thread().ident}")      response = requests.get("https://www.lbc.co.uk")      logger.info(f"Task 3 status code: {response.status_code}")      logger.info("Task 3 completed")   # Execute tasks asynchronously  if __name__ == "__main__":     print("Asynchronous task execution:")      start_time = time.time()       with ThreadPoolExecutor(max_workers=3) as executor:          executor.submit(task1)          executor.submit(task2)          executor.submit(task3)       end_time = time.time()      total_time = end_time - start_time      logger.info(f"Total time taken: {total_time} seconds")
```


This program can now run faster as, while task1() is waiting to finish, task2() can start, and so on. In the terminal output shown below, one particular execution ran in around 0.18s. New threads have been created here to have multiple, separate flows of execution.


Press enter or click to view image in full size


Python threading can be used to decrease the time taken for a program to execute, by allowing tasks to run concurrently. The global interpreter lock restricts the potential of multithreading in Python, as it does not allow multiple threads to run two different processes at exactly the same time. Instead, the threads take turns running a process to speed up the overall program.


However, the global interpreter lock is released for I/O operations (such as web requests, like in the example above). So, multithreading can be used to run I/O operations concurrently.


An issue that can arise with multithreading is a race condition. Race conditions can occur when multiple threads attempt to make changes to a Python object at the same time. This can cause data to be changed incorrectly, introducing errors and potentially security vulnerabilities into the code. This can be avoided by synchronising access to shared data, using a lock, for example.


In summary, multithreading is one method of reducing the time taken for a Python process to execute. ThreadPoolExecutor can be used to introduce multiple threads, which can allow different parts of the program to run asynchronously.
