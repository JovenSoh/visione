FROM python:3.10

COPY . /usr/src/app
WORKDIR /usr/src/app

# Install NumPy first to avoid binary compatibility issues
RUN pip install --no-cache-dir --upgrade pip \
    && pip uninstall numpy \
    && pip install --no-cache-dir -r requirements.txt

ENV PYTHONPATH "${PYTHONPATH}:/usr/src/"
