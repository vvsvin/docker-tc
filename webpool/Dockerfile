
FROM centos:7 as base

# -------------------------------------------------------------
# Env variables
# -------------------------------------------------------------
ENV SIEMENS_DIR=/apps/siemens \
    TCDATA_DIR=/data/tcdata \
    INSTALL_DIR=/apps/installers \
    WEBAPPS_DIR=/apps/webapps \
    SETUP_TM_FILE=apache-tomcat-8.5.57.tar.gz \
    SETUP_OS_FILE=runOS-webpool.sh \
    RUN_TC_FILE=runTcWebPool.sh \
    SETENV_FILE=setenv.sh \
    TC_PROFILE_FILE=tcprofile \
    JAVA_HOME=/etc/alternatives/jre \
    TC_ROOT=/apps/siemens/tc12.2.0.4 \
    TC_DATA=/data/tcdata \
    SPLM_LICENSE_SERVER=28000@docker-host \
    TC_LOG=/data/tclogs \
    TC_LOGS=/data/tclogs \
    TC_TMP_DIR=/data/tclogs \
    WEB_DESC_FILE=tc.xml \
    AWC_DESC_FILE=awc.xml

COPY $SETUP_OS_FILE $INSTALL_DIR/

RUN chmod ug+x $INSTALL_DIR/*.sh && \
    sync && \
    $INSTALL_DIR/$SETUP_OS_FILE && \
    rm -rf $INSTALL_DIR

#EXPOSE 8080
#EXPOSE 8081

COPY $SETUP_TM_FILE $WEBAPPS_DIR/

RUN cd $WEBAPPS_DIR && tar zxvf $SETUP_TM_FILE && \
    ln -s apache-tomcat-8.5.57 tomcat && \
    rm -f $SETUP_TM_FILE

COPY $TC_PROFILE_FILE $RUN_TC_FILE /usr/local/bin/
COPY $SETENV_FILE $WEBAPPS_DIR/tomcat/bin/
COPY $WEB_DESC_FILE $AWC_DESC_FILE $WEBAPPS_DIR/tomcat/conf/Catalina/localhost/


RUN chown infodba:dba /usr/local/bin/$TC_PROFILE_FILE && \
    chown infodba:dba /usr/local/bin/$RUN_TC_FILE && \
    chown -R infodba:dba $WEBAPPS_DIR && \
    chmod ug+x /usr/local/bin/$TC_PROFILE_FILE && \
    chmod ug+x /usr/local/bin/$RUN_TC_FILE

# shared from host
#COPY tcroot
#COPY tcdata

USER infodba
WORKDIR /home/infodba

# Default command for runtime
#CMD $WEBAPPS_DIR/tomcat/bin/startup.sh ; $TC_ROOT/pool_manager/confs/tcua12/mgrstart ; /bin/bash
CMD /usr/local/bin/$RUN_TC_FILE ; /bin/bash


