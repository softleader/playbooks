FROM ubuntu:16.04

RUN apt-get update && \
    apt-get install -y software-properties-common && \
    apt-add-repository ppa:ansible/ansible && \
    apt-get update && \
    apt-get install -y ansible && \
    apt-get install -y make

WORKDIR /playbooks

COPY group_vars/ ./group_vars/
COPY roles/ ./roles/
COPY ansible.cfg ./
COPY *.yml ./
COPY Makefile ./