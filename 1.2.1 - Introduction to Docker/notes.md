# Module 1

# 1.2.1 Introduction to Docker

- Docker allows us to run images in containers
    - Images are executable packages we can reproduce
    - Containers are running images
- A common example is to have a container running Ubuntu.
    - We can install any packages in this virtual container
    - We can specify replicable requirements for the image
- Another example is running a database in the container
- In the context of data engineering, containers enable us to replicate our pipelines in the cloud

## Creating a custom pipeline with docker

`pipeline.py`

```python
import sys
import pandas # we don't need this but it's useful for the example

# print arguments
print(sys.argv)

# argument 0 is the name os the file
# argumment 1 contains the actual first argument we care about
day = sys.argv[1]

# cool pandas stuff goes here

# print a sentence with the argument
print(f'job finished successfully for day = {day}')
```

`Dockerfile`

```python
# base Docker image that we will build on
FROM python:3.9.1

# set up our image by installing prerequisites; pandas in this case
RUN pip install pandas

# set up the working directory inside the container
WORKDIR /app
# copy the script to the container. 1st name is source file, 2nd is destination
COPY pipeline.py pipeline.py

# define what to do first when the container runs
# in this example, we will just run the script
ENTRYPOINT ["python", "pipeline.py"]
```

### Building the image

```python
docker build -t test:pandas .
```

### Running the container

```python
docker run -it test:pandas some_number
```

<aside>
📌 Note: these instructions asume that `pipeline.py` and `Dockerfile` are in the same directory. The Docker commands should also be run from the same directory as these files.

</aside>