# # Use the official Python image as a base image
# FROM python:3.10-slim 

# # Set environment variables
# ENV POETRY_HOME="/opt/poetry" \ 
#     PYSETUP_PATH="/app" \   
#     PIP_NO_CACHE_DIR=off \  
#     POETRY_VIRTUALENVS_PATH="/app/.venv"    

# # Prepend Poetry and the virtual environment to the PATH
# ENV PATH="$POETRY_HOME/bin:$POETRY_VIRTUALENVS_PATH/bin:$PATH"
    
# # Update package list and install dependencies for Poetry and building Python packages
# RUN apt-get update \
#     && apt-get install --no-install-recommends -y \
#         curl \             
#         bash \             
#         build-essential    

# # Install Poetry
# RUN curl -sSL https://install.python-poetry.org | python3 -
    
# # Verify the installation of Poetry
# RUN poetry --version

# # Configure Poetry to not create virtual environments
# RUN poetry config virtualenvs.create false

# # Uncomment and set your internal repository URL if needed
# # RUN poetry config repositories.internal https://<URL>

# # Set the working directory
# WORKDIR $PYSETUP_PATH

# # Copy only the necessary files for installing dependencies first
# COPY pyproject.toml poetry.lock ./

# # Set PYTHONPATH environment variable
# ENV PYTHONPATH=/app

# # Install dependencies using Poetry
# RUN poetry install 

# # Copy the rest of the application code to the container
# COPY . $PYSETUP_PATH

# # Set PYTHONPATH environment variable again (redundant)
# ENV PYTHONPATH=/app

# # Expose the port that FastAPI will run on
# EXPOSE 8000

# # Run the prestart script and start the FastAPI server
# CMD ["bash", "-c", "poetry run bash ./prestart.sh && poetry run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000"]


# Use the official Python image as the base image
FROM python:3.11-slim

# Set the working directory inside the container
WORKDIR /app

# Install Poetry
RUN pip install poetry

# Copy the pyproject.toml and poetry.lock to the working directory
COPY pyproject.toml poetry.lock ./

# Install the project dependencies
RUN poetry install --no-root --no-dev

# Install PostgreSQL client libraries
RUN apt-get update && apt-get install -y libpq-dev

# Copy the rest of the application code to the working directory
COPY . .

# Expose port 8000 to access the application
EXPOSE 8000

# Command to run the Uvicorn server
CMD ["poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
