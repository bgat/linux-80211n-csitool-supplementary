all: print_packets get_first_bfee parse_log log_to_file nl_bf_to_eff

KERNEL = $(strip $(shell uname -r))
KERNEL_SOURCE = /lib/modules/$(KERNEL)/build

ifneq ($(wildcard $(KERNEL_SOURCE)/include/uapi),)
        KERNEL_HEADERS = $(KERNEL_SOURCE)/include/uapi
else ifneq ($(wildcard $(KERNEL_SOURCE)/include),)
        KERNEL_HEADERS = $(KERNEL_SOURCE)/include
else
        $(error Kernel headers not found)
endif

CFLAGS = -Wall -Werror
LDLIBS = -lm
CC = gcc

nl_bf_to_eff: nl_bf_to_eff.c bf_to_eff.o iwl_nl.o util.o q_approx.o

log_to_file.c: iwl_connector.h

iwl_nl.c: iwl_connector.h

iwl_connector.h: connector_users.h

connector_users.h: $(KERNEL_HEADERS)/linux/connector.h
	echo "#undef CN_NETLINK_USERS" > connector_users.h
	grep "#define CN_NETLINK_USERS" $(KERNEL_HEADERS)/linux/connector.h >> connector_users.h

PKG = ../debian/linux-80211n-csitool-supplementary
DESTDIR = $(PKG)/usr/sbin

clean:
	rm -f *.o get_first_bfee log_to_file print_packets parse_log nl_bf_to_eff connector_users.h

install:
	mkdir -p $(DESTDIR)
	cp print_packets $(DESTDIR)/csitool-print_packets
	cp get_first_bfee $(DESTDIR)/csitool-get_first_bfee
	cp parse_log $(DESTDIR)/csitool-parse_log
	cp log_to_file $(DESTDIR)/csitool-log_to_file
	cp nl_bf_to_eff $(DESTDIR)/csitool-nl_bf_to_eff
