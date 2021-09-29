FROM registry.access.redhat.com/ubi8/openjdk-8-runtime:1.10-1
MAINTAINER Frédéric Giloux
LABEL jmeter.version="5.4"

# Set JMeter base
ENV JMETER_BASE /opt/jmeter

# Create jmeter directory with tests and results folder and install JMeter
RUN mkdir -p $JMETER_BASE/{tests,results} \
    && curl -SL https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.4.tgz \
    | tar -xzC $JMETER_BASE \
    && rm -rf apache-jmeter-5.4.tgz

# If you don't have internet access from the build machine use COPY instead of curl:
# COPY binaries/apache-jmeter-4.0.tgz $JMETER_BASE

############# ARE PLUGINS NEEDED?
# COPY binaries/JMeterPlugins-ExtrasLibs-1.4.0.zip $JMETER_BASE/apache-jmeter-4.0/

# Set JMeter home
ENV JMETER_HOME $JMETER_BASE/apache-jmeter-5.4

# Add JMeter to the Path
ENV PATH $JMETER_HOME/bin:$PATH

# Additional jars (ex. AMQP driver) can be copied into $JMETER_HOME/lib
COPY lib/* $JMETER_HOME/lib/

# Copy custom user.properties file for reports dashboard to be generated
# COPY user.properties $JMETER_HOME/bin/user.properties

# Set working directory
WORKDIR $JMETER_BASE

COPY scripts/run.sh $JMETER_BASE/run.sh

USER 0

RUN chgrp -R 0 $JMETER_BASE \
    && chmod -R g+rw $JMETER_BASE \
    && chmod +x $JMETER_BASE/run.sh

USER 1001

ENTRYPOINT $JMETER_BASE/run.sh
