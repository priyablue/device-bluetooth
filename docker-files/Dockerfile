###############################################################################
# Copyright 2016-2017 Dell Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
###############################################################################
FROM alpine:3.4
MAINTAINER Tyler Cox <tyler_cox@dell.com>

# environment variables
ENV APP_DIR=/edgex/edgex-device-bluetooth
ENV APP=device-bluetooth.jar
ENV APP_PORT=49988
ENV ARCHIVE="EdgeXResourcePyGatt-*"
ENV PY_APP_DIR=/edgex/edgex-resource-pygatt
ENV PY_APP_PORT=5000
ENV VIRTUAL_ENV=$PY_APP_DIR/virtualenv
ENV PY_APP="$VIRTUAL_ENV/bin/FlaskController.py"

RUN mkdir -p $PY_APP_DIR

RUN apk --update add openjdk8-jre
RUN apk --no-cache add python ca-certificates
RUN apk add --update py-virtualenv build-base

#copy JAR and property files to the image
COPY *.jar $APP_DIR/$APP
COPY $ARCHIVE $PY_APP_DIR/
COPY docker-files/*.properties $APP_DIR/
COPY docker-files/*.sh $APP_DIR/

RUN mkdir -p "$VIRTUAL_ENV"
RUN virtualenv "$VIRTUAL_ENV"
RUN source "$VIRTUAL_ENV/bin/activate" && pip install $PY_APP_DIR/$ARCHIVE

# give it executable permissions
RUN chmod 755 $APP_DIR/bluetooth.sh

#expose ports
EXPOSE $APP_PORT
EXPOSE $PY_APP_PORT

#set the working directory
WORKDIR $APP_DIR

RUN ls $APP_DIR
RUN ls $PY_APP_DIR

#kick off the micro service
ENTRYPOINT $APP_DIR/bluetooth.sh $APP -Djava.security.egd=file:/dev/urandom -Xmx100M $PY_APP $PY_APP_PORT