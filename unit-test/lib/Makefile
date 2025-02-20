
LIB_DIR = ../../lib
OBJ_DIR = ./obj
BIN_DIR = ./bin
LIB_SRCS = $(sort $(wildcard $(LIB_DIR)/*.c))
LIB_OBJS = $(LIB_SRCS:$(LIB_DIR)/%.c=$(OBJ_DIR)/%.o)
LIB_DEPS = $(LIB_SRCS:$(LIB_DIR)/%.c=$(OBJ_DIR)/%.d)

MOCK_DIR = ../mock
MOCK_SRCS = $(sort $(wildcard $(MOCK_DIR)*.c))
MOCK_OBJS += $(OBJ_DIR)/hw_alloc_assoc_write.o
MOCK_OBJS += $(OBJ_DIR)/hw_alloc_assoc_read.o

TESTS_SRCS = $(sort $(wildcard test_*.c))
TESTS = $(TESTS_SRCS:%.c=$(BIN_DIR)/%)


LDFLAGS = -L$(MOCK_DIR) -lpthread -lcmocka -lpqosmock -z noexecstack -z relro -z now
CFLAGS = -pthread -I$(LIB_DIR) -I$(MOCK_DIR) \
	-D_GNU_SOURCE \
	-W -Wall -Wextra -Wstrict-prototypes \
	-Wold-style-definition -Wpointer-arith \
	-Wcast-qual -Wundef -Wwrite-strings \
	-Wformat -Wformat-security -fstack-protector \
	-Wunreachable-code -Wsign-compare -Wno-endif-labels \
	-D'PQOS_LOCAL=' -D'PQOS_API='

CFLAGS += -g -ggdb -O0

all: $(TESTS)

$(OBJ_DIR)/%.o: $(LIB_DIR)/%.c $(OBJ_DIR)/%.d
	$(CC) $(CFLAGS) -c $< -o $@

$(OBJ_DIR)/allocation.o: $(LIB_DIR)/allocation.c $(OBJ_DIR)/allocation.d
	$(CC) $(CFLAGS) -c $< -o $@
	objcopy $@ \
		--globalize-symbol=hw_alloc_assoc_write --weaken-symbol=hw_alloc_assoc_write \
		--globalize-symbol=hw_alloc_assoc_read --weaken-symbol=hw_alloc_assoc_read \
		--globalize-symbol=hw_alloc_reset_cos --weaken-symbol=hw_alloc_reset_cos \
		--globalize-symbol=hw_alloc_reset_l2cdp --weaken-symbol=hw_alloc_reset_l2cdp \
		--globalize-symbol=hw_alloc_reset_l3cdp --weaken-symbol=hw_alloc_reset_l3cdp \
		--globalize-symbol=hw_alloc_reset_assoc --weaken-symbol=hw_alloc_reset_assoc \
		--globalize-symbol=hw_alloc_assoc_unused --weaken-symbol=hw_alloc_assoc_unused \
		--globalize-symbol=hw_l3ca_set --weaken-symbol=hw_l3ca_set \
		--globalize-symbol=hw_l3ca_set --weaken-symbol=hw_l3ca_set \
		--globalize-symbol=hw_l3ca_get --weaken-symbol=hw_l3ca_get \
		--globalize-symbol=hw_l2ca_set --weaken-symbol=hw_l2ca_set \
		--globalize-symbol=hw_l2ca_get --weaken-symbol=hw_l2ca_get \
		$@

$(OBJ_DIR)/hw_monitoring.o: $(LIB_DIR)/hw_monitoring.c $(OBJ_DIR)/hw_monitoring.d
	$(CC) $(CFLAGS) -c $< -o $@
	objcopy $@ \
		--globalize-symbol=hw_mon_assoc_write --weaken-symbol=hw_mon_assoc_write \
		--globalize-symbol=hw_mon_assoc_read --weaken-symbol=hw_mon_assoc_read \
		--globalize-symbol=hw_mon_assoc_unused --weaken-symbol=hw_mon_assoc_unused \
		--globalize-symbol=hw_mon_read --weaken-symbol=hw_mon_read \
		--globalize-symbol=hw_mon_start_perf --weaken-symbol=hw_mon_start_perf \
		--globalize-symbol=hw_mon_stop_perf --weaken-symbol=hw_mon_stop_perf \
		--globalize-symbol=hw_mon_start_counter --weaken-symbol=hw_mon_start_counter \
		--globalize-symbol=hw_mon_read_counter --weaken-symbol=hw_mon_read_counter \
		$@

$(OBJ_DIR)/hw_cap.o: $(LIB_DIR)/hw_cap.c $(OBJ_DIR)/hw_cap.d
	$(CC) $(CFLAGS) -c $< -o $@
	objcopy $@ \
		--globalize-symbol=hw_cap_l3ca_cdp --weaken-symbol=hw_cap_l3ca_cdp \
		--globalize-symbol=hw_cap_l2ca_cdp --weaken-symbol=hw_cap_l2ca_cdp \
		$@

$(OBJ_DIR)/resctrl_alloc.o: $(LIB_DIR)/resctrl_alloc.c $(OBJ_DIR)/resctrl_alloc.d
	$(CC) $(CFLAGS) -c $< -o $@
	objcopy $@ \
		--globalize-symbol=resctrl_alloc_fopen --weaken-symbol=resctrl_alloc_fopen \
		--globalize-symbol=resctrl_alloc_fclose --weaken-symbol=resctrl_alloc_fclose \
		$@

$(OBJ_DIR)/os_allocation.o: $(LIB_DIR)/os_allocation.c $(OBJ_DIR)/os_allocation.d
	$(CC) $(CFLAGS) -c $< -o $@
	objcopy $@ \
		--globalize-symbol=os_alloc_assoc_set --weaken-symbol=os_alloc_assoc_set \
		--globalize-symbol=os_alloc_assoc_get --weaken-symbol=os_alloc_assoc_get \
		--globalize-symbol=os_alloc_assoc_set_pid --weaken-symbol=os_alloc_assoc_set_pid \
		--globalize-symbol=os_alloc_assoc_get_pid --weaken-symbol=os_alloc_assoc_get_pid \
		--globalize-symbol=os_alloc_reset_cores --weaken-symbol=os_alloc_reset_cores \
		--globalize-symbol=os_alloc_reset_schematas --weaken-symbol=os_alloc_reset_schematas \
		--globalize-symbol=os_alloc_reset_tasks --weaken-symbol=os_alloc_reset_tasks \
		--globalize-symbol=os_alloc_mount --weaken-symbol=os_alloc_mount \
		$@

$(OBJ_DIR)/os_cap.o: $(LIB_DIR)/os_cap.c $(OBJ_DIR)/os_cap.d
	$(CC) $(CFLAGS) -c $< -o $@
	objcopy $@ \
		--globalize-symbol=os_cap_mon_resctrl_support --weaken-symbol=os_cap_mon_resctrl_support \
		--globalize-symbol=os_cap_mon_perf_support --weaken-symbol=os_cap_mon_perf_support \
		$@

$(LIB_DEPS): $(OBJ_DIR)/%.d : $(LIB_DIR)/%.c
	mkdir -p $(OBJ_DIR)
	$(CC) -MM -MP -MF $@ $(CFLAGS) $<

%.o: %.c %.d

%.d: %.c
	$(CC) -MM -MP -MF $@ $(CFLAGS) $<

$(BIN_DIR)/test_api: test_api.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_check_init \
		-Wl,--wrap=_pqos_api_lock \
		-Wl,--wrap=_pqos_api_unlock \
		-Wl,--wrap=hw_alloc_assoc_set \
		-Wl,--wrap=os_alloc_assoc_set \
		-Wl,--wrap=hw_alloc_assoc_get \
		-Wl,--wrap=os_alloc_assoc_get \
		-Wl,--wrap=os_alloc_assoc_set_pid \
		-Wl,--wrap=os_alloc_assoc_get_pid \
		-Wl,--wrap=hw_alloc_assoc_set \
		-Wl,--wrap=os_alloc_assoc_set \
		-Wl,--wrap=hw_alloc_assign \
		-Wl,--wrap=os_alloc_assign \
		-Wl,--wrap=hw_alloc_release \
		-Wl,--wrap=os_alloc_release \
		-Wl,--wrap=os_alloc_assign_pid \
		-Wl,--wrap=os_alloc_release_pid \
		-Wl,--wrap=hw_alloc_reset \
		-Wl,--wrap=os_alloc_reset \
		-Wl,--wrap=os_pid_get_pid_assoc \
		-Wl,--wrap=hw_l3ca_set \
		-Wl,--wrap=os_l3ca_set \
		-Wl,--wrap=hw_l3ca_get \
		-Wl,--wrap=os_l3ca_get \
		-Wl,--wrap=hw_l3ca_get_min_cbm_bits \
		-Wl,--wrap=os_l3ca_get_min_cbm_bits \
		-Wl,--wrap=hw_l2ca_set \
		-Wl,--wrap=os_l2ca_set \
		-Wl,--wrap=hw_l2ca_get \
		-Wl,--wrap=os_l2ca_get \
		-Wl,--wrap=hw_l2ca_get_min_cbm_bits \
		-Wl,--wrap=os_l2ca_get_min_cbm_bits \
		-Wl,--wrap=hw_mba_set \
		-Wl,--wrap=os_mba_set \
		-Wl,--wrap=hw_mba_get \
		-Wl,--wrap=os_mba_get \
		-Wl,--wrap=cpuinfo_get_config \
		-Wl,--wrap=hw_mon_reset \
		-Wl,--wrap=os_mon_reset \
		-Wl,--wrap=hw_mon_assoc_get \
		-Wl,--wrap=hw_mon_start \
		-Wl,--wrap=os_mon_start \
		-Wl,--wrap=hw_mon_stop \
		-Wl,--wrap=os_mon_stop \
		-Wl,--wrap=pqos_mon_poll_events \
		-Wl,--wrap=os_mon_start_pids \
		-Wl,--wrap=os_mon_add_pids \
		-Wl,--wrap=os_mon_remove_pids \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=_pqos_inter_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_allocation: test_hw_allocation.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=cpuinfo_get_config \
		-Wl,--wrap=_pqos_cap_l3cdp_change \
		-Wl,--wrap=_pqos_cap_l2cdp_change \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_assoc_write: test_hw_alloc_assoc_write.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_assoc_read: test_hw_alloc_assoc_read.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_reset_l3cdp: test_hw_alloc_reset_l3cdp.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_reset_l2cdp: test_hw_alloc_reset_l2cdp.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_reset_assoc: test_hw_alloc_reset_assoc.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_assoc_unused: test_hw_alloc_assoc_unused.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_l3ca_get_min_cbm_bits: test_hw_l3ca_get_min_cbm_bits.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_l2ca_get_min_cbm_bits: test_hw_l2ca_get_min_cbm_bits.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_reset_cos: test_hw_alloc_reset_cos.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_alloc_assign: test_hw_alloc_assign.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_mon_assoc_write: test_hw_mon_assoc_write.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=perf_mon_init \
		-Wl,--wrap=perf_mon_fini \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_mon_assoc_read: test_hw_mon_assoc_read.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=perf_mon_init \
		-Wl,--wrap=perf_mon_fini \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_mon_assoc_unused: test_hw_mon_assoc_unused.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=perf_mon_init \
		-Wl,--wrap=perf_mon_fini \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_mon_read: test_hw_mon_read.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_write \
		-Wl,--wrap=msr_read \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_mon_read_counter: test_hw_mon_read_counter.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=perf_mon_init \
		-Wl,--wrap=perf_mon_fini \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_mon_start_perf: test_hw_mon_start_perf.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=perf_mon_init \
		-Wl,--wrap=perf_mon_fini \
		-Wl,--wrap=perf_mon_is_event_supported \
		-Wl,--wrap=perf_mon_start \
		-Wl,--wrap=perf_mon_stop \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_mon_start_counter: test_hw_mon_start_counter.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=perf_mon_init \
		-Wl,--wrap=perf_mon_fini \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_monitoring: test_hw_monitoring.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=perf_mon_init \
		-Wl,--wrap=perf_mon_fini \
		-Wl,--wrap=perf_mon_is_event_supported \
		-Wl,--wrap=perf_mon_start \
		-Wl,--wrap=perf_mon_stop \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_cap: test_hw_cap.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=lcpuid \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=msr_write \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_cap_l3ca_cdp: test_hw_cap_l3ca_cdp.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=msr_write \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_hw_cap_l2ca_cdp: test_hw_cap_l2ca_cdp.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=msr_read \
		-Wl,--wrap=msr_write \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_resctrl_schemata: test_resctrl_schemata.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=fread \
		-Wl,--wrap=fgets \
		-Wl,--wrap=fprintf \
		-Wl,--wrap=fwrite \
		-Wl,--wrap=fputc \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_resctrl_alloc: test_resctrl_alloc.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=setvbuf \
		-Wl,--wrap=kill \
		-Wl,--wrap=resctrl_cpumask_write \
		-Wl,--wrap=resctrl_cpumask_read \
		-Wl,--wrap=resctrl_cpumask_set \
		-Wl,--wrap=resctrl_schemata_read \
		-Wl,--wrap=resctrl_schemata_l3ca_write \
		-Wl,--wrap=resctrl_schemata_l2ca_write \
		-Wl,--wrap=resctrl_schemata_mba_write \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_os_alloc_mount: test_os_alloc_mount.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=resctrl_mount \
		-Wl,--wrap=resctrl_umount \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@


$(BIN_DIR)/test_os_allocation: test_os_allocation.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=resctrl_mount \
		-Wl,--wrap=resctrl_umount \
		-Wl,--wrap=resctrl_lock_exclusive \
		-Wl,--wrap=resctrl_lock_release \
		-Wl,--wrap=resctrl_lock_shared \
		-Wl,--wrap=resctrl_alloc_init \
		-Wl,--wrap=resctrl_alloc_fini \
		-Wl,--wrap=resctrl_alloc_assoc_set \
		-Wl,--wrap=resctrl_alloc_assoc_get \
		-Wl,--wrap=resctrl_alloc_assoc_get_pid \
		-Wl,--wrap=resctrl_alloc_get_unused_group \
		-Wl,--wrap=resctrl_alloc_cpumask_read \
		-Wl,--wrap=resctrl_alloc_cpumask_write \
		-Wl,--wrap=resctrl_alloc_schemata_read \
		-Wl,--wrap=resctrl_alloc_schemata_write \
		-Wl,--wrap=resctrl_alloc_task_write \
		-Wl,--wrap=resctrl_cpumask_set \
		-Wl,--wrap=resctrl_schemata_l3ca_set \
		-Wl,--wrap=resctrl_schemata_l3ca_get \
		-Wl,--wrap=resctrl_schemata_l2ca_set \
		-Wl,--wrap=resctrl_schemata_l2ca_get \
		-Wl,--wrap=resctrl_schemata_mba_set \
		-Wl,--wrap=resctrl_schemata_mba_get \
		-Wl,--wrap=_pqos_cap_l3cdp_change \
		-Wl,--wrap=_pqos_cap_l2cdp_change \
		-Wl,--wrap=_pqos_cap_mba_change \
		-Wl,--wrap=mkdir \
		-Wl,--wrap=pqos_file_exists \
		-Wl,--wrap=pqos_dir_exists \
		-Wl,--wrap=resctrl_mon_active \
		-Wl,--wrap=pqos_fread_uint64 \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_os_alloc_assoc_set: test_os_alloc_assoc_set.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=resctrl_lock_exclusive \
		-Wl,--wrap=resctrl_lock_release \
		-Wl,--wrap=resctrl_lock_shared \
		-Wl,--wrap=resctrl_alloc_assoc_set \
		-Wl,--wrap=resctrl_mon_assoc_set \
		-Wl,--wrap=resctrl_mon_assoc_get \
		-Wl,--wrap=resctrl_mon_assoc_get_pid \
		-Wl,--wrap=resctrl_mon_assoc_set_pid \
		-Wl,--wrap=resctrl_alloc_assoc_set_pid \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_os_alloc_reset: test_os_alloc_reset.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=resctrl_mount \
		-Wl,--wrap=resctrl_umount \
		-Wl,--wrap=resctrl_lock_exclusive \
		-Wl,--wrap=resctrl_lock_release \
		-Wl,--wrap=resctrl_lock_shared \
		-Wl,--wrap=resctrl_cpumask_set \
		-Wl,--wrap=resctrl_alloc_cpumask_read \
		-Wl,--wrap=resctrl_alloc_cpumask_write \
		-Wl,--wrap=resctrl_alloc_schemata_write \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_os_cap: test_os_cap.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=_pqos_cap_get \
		-Wl,--wrap=resctrl_mount \
		-Wl,--wrap=resctrl_umount \
		-Wl,--wrap=resctrl_lock_exclusive \
		-Wl,--wrap=resctrl_lock_release \
		-Wl,--wrap=resctrl_lock_shared \
		-Wl,--wrap=pqos_dir_exists \
		-Wl,--wrap=pqos_file_exists \
		-Wl,--wrap=pqos_file_contains \
		-Wl,--wrap=pqos_fread_uint64 \
		-Wl,--wrap=resctrl_alloc_get_num_closids \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

$(BIN_DIR)/test_os_cap_mon: test_os_cap_mon.c $(LIB_OBJS)
	mkdir -p $(BIN_DIR)
	$(CC) $(CFLAGS) \
		-Wl,--wrap=pqos_dir_exists \
		-Wl,--wrap=pqos_file_exists \
		-Wl,--wrap=pqos_file_contains \
		-Wl,--wrap=pqos_fread_uint64 \
		-Wl,--wrap=resctrl_mount \
		-Wl,--start-group \
		$(LDFLAGS) $(LIB_OBJS) $< -Wl,--end-group -o $@

.PHONY: run
run: $(TESTS)
	@echo "Running Library Unit Tests"
	@for test in $(TESTS); do \
		bash -c "LD_LIBRARY_PATH=$(MOCK_DIR) ./$$test" || true; \
	done;

CHECKPATCH?=checkpatch.pl
.PHONY: checkpatch
checkpatch:
	$(CHECKPATCH) --no-tree --no-signoff --emacs \
	--ignore CODE_INDENT,INITIALISED_STATIC,LEADING_SPACE,SPLIT_STRING,\
	NEW_TYPEDEFS,UNSPECIFIED_INT,BLOCK_COMMENT_STYLE,\
	SPDX_LICENSE_TAG,ARRAY_SIZE,EMBEDDED_FUNCTION_NAME,\
	SYMBOLIC_PERMS,CONST_STRUCT \
	-f test_api.c \
	-f test_hw_allocation.c

CLANGFORMAT?=clang-format
.PHONY: clang-format
clang-format:
	@for file in $(wildcard *.[ch]); do \
		echo "Checking style $$file"; \
		$(CLANGFORMAT) -style=file "$$file" | diff "$$file" - | tee /dev/stderr | [ $$(wc -c) -eq 0 ] || \
		{ echo "ERROR: $$file has style problems"; exit 1; } \
	done

CODESPELL?=codespell
.PHONY: codespell
codespell:
	$(CODESPELL) . --skip $(OBJ_DIR) -q 2

.PHONY: style
style:
	$(MAKE) checkpatch
	$(MAKE) clang-format
	$(MAKE) codespell

clean:
	rm -rf $(BIN_DIR)
	rm -rf $(OBJ_DIR)