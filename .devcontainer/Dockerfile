#ref: https://docs.aws.amazon.com/sagemaker/latest/dg/adapt-training-container.html

FROM python:3.10.6-bullseye

ARG spark_version="3.3.0"
ARG hadoop_version="3"
ARG scala_version
ARG spark_checksum="1e8234d0c1d2ab4462d6b0dfe5b54f2851dcd883378e0ed756140e10adfb5be4123961b521140f580e364c239872ea5a9f813a20b73c69cb6d4e95da2575c29c"
ARG openjdk_version="17"

ENV APACHE_SPARK_VERSION="${spark_version}" \
    HADOOP_VERSION="${hadoop_version}"

RUN apt-get update && apt-get install -y --no-install-recommends \
        "openjdk-${openjdk_version}-jre-headless" \
        ca-certificates-java && \
        apt-get clean && rm -rf /var/lib/apt/lists/*

# Spark installation
WORKDIR /tmp

RUN if [ -z "${scala_version}" ]; then \
    wget -qO "spark.tgz" "https://archive.apache.org/dist/spark/spark-${APACHE_SPARK_VERSION}/spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz"; \
  else \
    wget -qO "spark.tgz" "https://archive.apache.org/dist/spark/spark-${APACHE_SPARK_VERSION}/spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}-scala${scala_version}.tgz"; \
  fi && \
  echo "${spark_checksum} *spark.tgz" | sha512sum -c - && \
  tar xzf "spark.tgz" -C /usr/local --owner root --group root --no-same-owner && \
  rm "spark.tgz"

# Configure Spark
ENV SPARK_HOME=/usr/local/spark
ENV SPARK_OPTS="--driver-java-options=-Xms1024M --driver-java-options=-Xmx4096M --driver-java-options=-Dlog4j.logLevel=info" \
    PATH="${PATH}:${SPARK_HOME}/bin"

RUN if [ -z "${scala_version}" ]; then \
    ln -s "spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}" "${SPARK_HOME}"; \
  else \
    ln -s "spark-${APACHE_SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}-scala${scala_version}" "${SPARK_HOME}"; \
  fi && \
  # Add a link in the before_notebook hook in order to source automatically PYTHONPATH && \
  mkdir -p /usr/local/bin/before-notebook.d && \
  ln -s "${SPARK_HOME}/sbin/spark-config.sh" /usr/local/bin/before-notebook.d/spark-config.sh


WORKDIR /

# Installing numpy, pandas, scikit-learn, scipy
RUN pip install --no-cache --upgrade \
            numpy \
            pandas \
            scikit-learn \
            hyperopt \
            mlflow \
            xgboost \
            pyarrow \
            pyspark \
            mlflow \
            jupyterlab \
            PyWavelets \ 
            keras-tcn

RUN pip install --upgrade pip

WORKDIR /


