# Custom PYNQ firmware and tools for RFSoCs

This organization contains a collection of firmware for RFSoC boards, files to build PYNQ distributions,
and tools to interact with them.

All firmware builds have an identical interface from the Python side, so the Python tools can be
used on _any_ board freely. In addition, the firmware is set up to allow using the RFSoC data from
HDL, rather than a block diagram view or from Matlab.

All of these repositories are Vivado GitHub managed repos in the style as 
described [here](https://github.com/barawn/verilog-library-barawn/wiki/GitHub-Managed-Vivado-Repositories).
Please read those details first.

The boards involved are:
* HiTech Global [HTG-ZRF-HH](https://www.hitechglobal.com/Boards/Zynq_RFSOC_PCIE_HS.htm), a half-height PCIe form factor card with a ZU28DR.
* Xilinx [ZCU111](https://www.xilinx.com/products/boards-and-kits/zcu111.html)
* Real Digital [RFSoC-4x2](https://www.xilinx.com/support/university/xup-boards/RFSoC4x2.html)
* Trenz Electronic [TEB0835](https://shop.trenz-electronic.de/en/TEB0835-02-A-PCIe-Baseboard-for-Trenz-Electronic-TE0835-RFSoC) using a ZU25DR, although a ZU47DR would just require a part number change.
* PUEO SURF (in progress)

## Images

https://pueo.uchicago.edu/data/pueo/disk-images/ 

## Common Interface

All of these repositories contain common elements, namely 4 internal RAMs that capture data when commanded to by the processing system via a GPIO,
as well as a DebugBridge module to allow for running an XVC server so that a JTAG cable is not needed.

The Python files describing the Overlay all have an ``internal_capture()`` function which captures data from those 4 internal RAMs using the GPIO.
This common functionality is used by the ``rfsoc-pydaq`` program.

# PYNQ notes

The "official" PYNQ repositories aren't very well maintained, as there are a number of known issues/patches which just get shoved
in commit notes or other repositories. So don't use them, use these.

# rfsoc-pydaq

``rfsoc-pydaq`` is a Tkinter-based Python interface for quickly interacting with an RFSoC. It is board-agnostic so long as the overlay supports
the ``internal_capture()`` function as mentioned above.

To use it, you need a directory containing:
* a Python module implementing a PYNQ Overlay class to control the board (see the ``python/`` subdirectories)
* the bitstream you plan on programming (which should be referenced _directly_ in the PYNQ Overlay class constructor, again, see examples in ``python/``)
* the hardware handoff (``.hwh``) file for that bitstream which should be named _identically_ to it (if bitstream is ``my_bitstream.bit`` you want ``my_bitstream.hwh``)
* any additional Python modules or files needed (e.g. clock programming modules/files)

