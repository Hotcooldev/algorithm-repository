FROM hbpmip/python-base-build:0.4.4

COPY requirements-dev.txt /requirements-dev.txt
RUN pip install -r /requirements-dev.txt

COPY requirements.txt /requirements.txt
RUN pip install -r /requirements.txt

# TODO: put these dependencies out of /python-sgd-regression
COPY sklearn_to_pfa /src/sklearn_to_pfa

COPY sgd_regression.py /src/sgd_regression.py
COPY tests/unit /src/tests/

WORKDIR /src
RUN pytest tests/


FROM hbpmip/python-mip:0.4.0

MAINTAINER mirco.nasuti@chuv.ch

ENV DOCKER_IMAGE=hbpmip/python-sgd-regression:0.0.7 \
    FUNCTION=python-sgd-regression

# TODO: git is required to install `git+https://github.com/jsonpickle/jsonpickle.git` for scikit-learn model
# serialization - we can remove it once jsonpickle==0.9.6 is available on PyPi
RUN apt-get update
RUN apt-get install -y git

COPY requirements.txt /requirements.txt
RUN pip install -r /requirements.txt

COPY sgd_regression.py /sgd_regression.py

# TODO: put these dependencies out of /python-sgd-regression
COPY sklearn_to_pfa /sklearn_to_pfa

ENTRYPOINT ["python", "/sgd_regression.py"]

LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.name="hbpmip/python-sgd-regression" \
      org.label-schema.description="Python implementation of sgd-regression" \
      org.label-schema.url="https://github.com/LREN-CHUV/algorithm-repository" \
      org.label-schema.vcs-type="git" \
      org.label-schema.vcs-url="https://github.com/LREN-CHUV/algorithm-repository.git" \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.version="$VERSION" \
      org.label-schema.vendor="LREN CHUV" \
      org.label-schema.license="AGPLv3" \
      org.label-schema.docker.dockerfile="Dockerfile" \
      org.label-schema.schema-version="1.0"