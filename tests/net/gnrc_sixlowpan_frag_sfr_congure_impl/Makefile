include ../Makefile.net_common

USEMODULE += auto_init_gnrc_netif
USEMODULE += gnrc_ipv6_router_default
USEMODULE += gnrc_sixlowpan_frag_sfr
USEMODULE += gnrc_udp
USEMODULE += gnrc_rpl
USEMODULE += auto_init_gnrc_rpl
USEMODULE += gnrc_pktdump
USEMODULE += gnrc_icmpv6_echo
USEMODULE += shell
USEMODULE += shell_cmds_default
USEMODULE += shell_cmd_gnrc_pktbuf
USEMODULE += shell_cmd_gnrc_udp
USEMODULE += ps
USEMODULE += netstats_l2
USEMODULE += netstats_ipv6
USEMODULE += netstats_rpl

ifeq (native, $(BOARD))
  USEMODULE += socket_zep
  USEMODULE += socket_zep_hello
  USEMODULE += netdev
  TERMFLAGS = -z 127.0.0.1:17754 # Murdock has no IPv6 support
else
  USEMODULE += netdev_default
  # automated test only works on native
  TESTS=
endif

CONGURE_IMPL ?= congure_sfr

ifeq (congure_abe,$(CONGURE_IMPL))
  USEMODULE += gnrc_sixlowpan_frag_sfr_congure_abe
else
ifeq (congure_quic,$(CONGURE_IMPL))
  USEMODULE += gnrc_sixlowpan_frag_sfr_congure_quic
  USEMODULE += ztimer_msec
else
ifeq (congure_reno,$(CONGURE_IMPL))
  USEMODULE += gnrc_sixlowpan_frag_sfr_congure_reno
else
ifeq (congure_sfr,$(CONGURE_IMPL))
  USEMODULE += gnrc_sixlowpan_frag_sfr_congure_sfr
else
  $(error "Unknown CongURE implementation `$(CONGURE_IMPL)`")
endif
endif
endif
endif

.PHONY: zep_dispatch

zep_dispatch:
	$(Q)env -u CC -u CFLAGS $(MAKE) -C $(RIOTTOOLS) $@

TERMDEPS += zep_dispatch

include $(RIOTBASE)/Makefile.include

# Set a custom channel if needed
include $(RIOTMAKE)/default-radio-settings.inc.mk
