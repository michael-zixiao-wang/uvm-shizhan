# ==============================================================================
# 《UVM实战》通用 Makefile
# 放置路径: ~/project/uvm-shizhan/vcs/Makefile
# ==============================================================================

# 1. 目标文件夹变量 (默认运行 2.2.1，可通过命令行传入 DIR=... 覆盖)
CHAP ?= ch2
SECTION ?= section2.2
SUBSECT ?= 2.2.1

DIR = $(CHAP)/$(SECTION)/$(SUBSECT)

# 2. 提取章节主目录 (利用 Make 字符串替换，从传入的路径提取首个目录名，如 ch2)
CH = $(firstword $(subst /, ,$(DIR)))

# 3. 定义路径
SRC_BASE   = ./src
TARGET_DIR = $(SRC_BASE)/$(DIR)
DUT_DIR    = $(SRC_BASE)/$(CH)/dut

# 4. 自动收集编译文件 (包含对应章节的 DUT 和 目标测试小节的验证环境)
TB_FILES   = $(wildcard $(TARGET_DIR)/*.sv $(TARGET_DIR)/*.v)
DUT_FILES  = $(wildcard $(DUT_DIR)/*.sv $(DUT_DIR)/*.v)

# 5. VCS 编译选项
# -sverilog       : 开启 SystemVerilog 支持
# -timescale      : 设置时间精度
# -ntb_opts uvm   : 使用 VCS 自带的 UVM 库 (版本通常兼容 1.1d)
# -full64         : 以 64 位模式运行
# -debug_access   : 开启全权限调试 (方便使用 Verdi 查看波形)
# +incdir+        : 指定头文件搜索路径，确保 `include "my_driver.sv" 能被找到
VCS = vcs
VCS_OPTS = -sverilog \
           -timescale=1ns/1ps \
           -ntb_opts uvm-1.1 \
           -full64 \
           -debug_access+all \
           +incdir+$(TARGET_DIR) \
           +incdir+$(DUT_DIR)

# 6. 仿真运行选项 (预留，日后可通过 UVM_TESTNAME 传参)
SIM_OPTS = 

.PHONY: all clean comp run

# 默认执行：清理 -> 编译 -> 仿真
all: clean comp run

# 编译步骤
comp:
	@echo "======================================================="
	@echo "=> [VCS COMPILING] Target Chapter : $(CH)"
	@echo "=> [VCS COMPILING] Target Dir     : $(TARGET_DIR)"
	@echo "=> [VCS COMPILING] DUT Dir        : $(DUT_DIR)"
	@echo "======================================================="
	$(VCS) $(VCS_OPTS) $(DUT_FILES) $(TB_FILES) -l comp.log

# 运行步骤
run:
	@echo "======================================================="
	@echo "=> [VCS RUNNING] Starting Simulation..."
	@echo "======================================================="
	./simv $(SIM_OPTS) -l sim.log

# 清理垃圾文件
clean:
	@echo "=> [CLEAN] Removing simulation output files..."
	rm -rf csrc simv simv.daidir *.log *.vpd *.fsdb ucli.key vc_hdrs.h DVEfiles
