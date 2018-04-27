# Step 3: Setting Platform Properties

After you complete the hardware platform design project in the Vivado Design
Suite, you must add platform properties (PFM) to define the platform name and
configure platform interfaces such as clocks, interrupts, and bus interfaces.
These properties are set once and stored in the project.

Platforms typically consists of multiple clocks. In this custom board, our
design contains four different clocks that are produced using a Clocking
Wizard. While only one of the clocks out of the four is enabled in this lab,
you can choose the desired clock frequency in SDx IDE for the hardware
functions to be accelerated. Likewise, AXI Ports to be used also need to be
identified and tagged with the Platform properties so the hardware functions
can use these ports to move data to and from the hardware functions. While
these AXI ports may or may not be visible in the block design used for
creating the hardware design in IP integrator, tagging these ports with
platform properties, makes them available for use by the hardware functions
later using SDx IDE.

  1. The Platform Identification property (PFM_NAME) must be set in the hardware design to define the Vendor, Library, Name, and Version (VLNV) of the platform. In the Tcl Console, type the following and press Enter. This sets the hardware platform name.

`set_property PFM_NAME "xilinx.com:zynq7_board:zynq7_board:1.0"\\[get_files
[get_property FILE_NAME [get_bd_designs]]]`

The PFM_NAME property needs the following syntax:

`<vendor>:<library>:<platform>:<version>`

  2. You can export any clock source with the platform, but for each clock you must also export synchronized reset signals using a Processor System Reset IP block in the platform. The `PFM.CLOCK` property can be set on BD cell, external port, or external interface. Accordingly, define clocks by typing the following Tcl commands in the Tcl Console and press Enter.
    
        set_property PFM.CLOCK { \
    	clk_out1 {id "2" is_default "true" proc_sys_reset "proc_sys_reset_0" } \
    	clk_out2 {id "1" is_default "false" proc_sys_reset "proc_sys_reset_1" } \
    	clk_out3 {id "0" is_default "false" proc_sys_reset "proc_sys_reset_2" } \
    	clk_out4 {id "3" is_default "false" proc_sys_reset "proc_sys_reset_3" } \
    	} [get_bd_cells /clk_wiz_0]
    

  3. Define the AXI ports in the design by typing the following in the TCL console and press Enter.
    
         set_property PFM.AXI_PORT { \
    	M_AXI_GP0 {memport "M_AXI_GP"} \
    	M_AXI_GP1 {memport "M_AXI_GP"} \
    	S_AXI_ACP {memport "S_AXI_ACP" sptag "ACP" memory "processing_system7_0 ACP_DDR_LOWOCM"} \
    	S_AXI_HP0 {memport "S_AXI_HP" sptag "HP0" memory "processing_system7_0 HP0_DDR_LOWOCM"} \
    	S_AXI_HP1 {memport "S_AXI_HP" sptag "HP1" memory "processing_system7_0 HP1_DDR_LOWOCM"} \
    	S_AXI_HP2 {memport "S_AXI_HP" sptag "HP2" memory "processing_system7_0 HP2_DDR_LOWOCM"} \
    	S_AXI_HP3 {memport "S_AXI_HP" sptag "HP3" memory "processing_system7_0 HP3_DDR_LOWOCM"} \
    	} [get_bd_cells /processing_system7_0]
    

  4. Interrupts must be connected to IP integrator Concat (xlconcat) blocks that are connected to the processing system. For Zynq®-7000 family it's the F2P_irq port. Type the following commands on the TCL console and press Enter.
    
         set intVar []
    for {set i 0} {$i < 16} {incr i} {
    	lappend intVar In$i {}
    }
    set_property PFM.IRQ $intVar [get_bd_cells /xlconcat_0]
    

  5. These properties can be viewed in the Properties windows by selecting the appropriate cells in the block design canvas. As an example, when you select the Concat cell in the canvas, you will see a PFM pull down menu in the Block Properties of the Concat block. When the drop down menu is expanded you will see the IRQ properties for your platform.

Figure: Looking up applied PFM Properties

![](egh1517376007023.image)

**Parent topic:** [Lab 1: Creating DSA for a Zynq-7000 AP SoC Processor Design](kdp1517356691922.html)

