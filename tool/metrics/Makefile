C_HEADERS    ?= ~/Projects/c_headers/include/
BTSTACK_ROOT ?= ../..

VPATH=${BTSTACK_ROOT}/src

INCLUDES= \
	-I ${BTSTACK_ROOT}/src \
	-I ${BTSTACK_ROOT}/3rd-party/micro-ecc \
	-I ${BTSTACK_ROOT}/3rd-party/rijndael \
	-I ${BTSTACK_ROOT}/platform/embedded \
	-I ${BTSTACK_ROOT}/platform/freertos \
	-I .

SRCS = \
	${BTSTACK_ROOT}/3rd-party/micro-ecc/uECC.c \
	${BTSTACK_ROOT}/3rd-party/rijndael/rijndael.c \
	${BTSTACK_ROOT}/src/ad_parser.c \
	${BTSTACK_ROOT}/src/ble/att_db.c \
	${BTSTACK_ROOT}/src/ble/att_db_util.c \
	${BTSTACK_ROOT}/src/ble/att_dispatch.c \
	${BTSTACK_ROOT}/src/ble/att_server.c \
	${BTSTACK_ROOT}/src/ble/gatt_client.c \
	${BTSTACK_ROOT}/src/ble/gatt-service/ancs_client.c \
	${BTSTACK_ROOT}/src/ble/gatt-service/battery_service_server.c \
	${BTSTACK_ROOT}/src/ble/gatt-service/battery_service_client.c \
	${BTSTACK_ROOT}/src/ble/gatt-service/device_information_service_server.c \
	${BTSTACK_ROOT}/src/ble/gatt-service/device_information_service_client.c \
	${BTSTACK_ROOT}/src/ble/le_device_db_tlv.c \
	${BTSTACK_ROOT}/src/ble/sm.c \
	${BTSTACK_ROOT}/src/btstack_crypto.c \
	${BTSTACK_ROOT}/src/btstack_linked_list.c \
	${BTSTACK_ROOT}/src/btstack_memory.c \
	${BTSTACK_ROOT}/src/btstack_memory_pool.c \
	${BTSTACK_ROOT}/src/btstack_run_loop.c \
	${BTSTACK_ROOT}/src/btstack_run_loop_base.c \
	${BTSTACK_ROOT}/src/btstack_tlv.c \
	${BTSTACK_ROOT}/src/btstack_util.c \
	${BTSTACK_ROOT}/src/hci.c \
	${BTSTACK_ROOT}/src/hci_cmd.c \
	${BTSTACK_ROOT}/src/hci_dump.c \
	${BTSTACK_ROOT}/src/hci_transport_h4.c \
	${BTSTACK_ROOT}/src/l2cap.c \
	${BTSTACK_ROOT}/src/l2cap_signaling.c \
	${BTSTACK_ROOT}/platform/embedded/btstack_stdin_embedded.c \
	${BTSTACK_ROOT}/platform/embedded/btstack_run_loop_embedded.c \
	${BTSTACK_ROOT}/platform/embedded/btstack_uart_block_embedded.c \

all: metrics.md metrics.html metrics.txt

metrics.tsv: ${SRCS}
	ccsm --disable-file --disable-global --exclude-std-headers --exclude-file=.h$$ --output-format=tsv --output-metrics=HIS_.* $^ -- -DuECC_NO_DEFAULT_RNG -isystem ${C_HEADERS}/c90 -isystem ${C_HEADERS}/c99 ${INCLUDES} > metrics_raw.tsv
	echo "# file\tfunction\tGOTO\tRETURN\t_\tSTMT \t_\tCCN\tPATH\tCALLS\tPARAM\tLEVEL\t_\t_\t_" > metrics_header.tsv
	cat metrics_header.tsv metrics_raw.tsv > $@
	rm metrics_header.tsv metrics_raw.tsv

metrics.md: metrics.tsv metrics_ccsm.py
	./metrics_ccsm.py > metrics.md

metrics.html: metrics.md
	pandoc --from markdown --to html -o metrics.html metrics.md

metrics.txt: metrics.tsv
	expand -t 70,160,170,180,190,210,220,230,240,250,260,270,280,290,300 metrics.tsv > metrics.txt

clean:
	rm -f metrics.tsv metrics.md metrics.txt metrics.html metrics_header.tsv metrics_raw.tsv
