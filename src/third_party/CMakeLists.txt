add_library(disasm STATIC
  libdisasm/ia32_implicit.c
	libdisasm/ia32_insn.c
	libdisasm/ia32_invariant.c
	libdisasm/ia32_modrm.c
	libdisasm/ia32_opcode_tables.c
	libdisasm/ia32_operand.c
	libdisasm/ia32_reg.c
	libdisasm/ia32_settings.c
	libdisasm/x86_disasm.c
	libdisasm/x86_format.c
	libdisasm/x86_imm.c
	libdisasm/x86_insn.c
	libdisasm/x86_misc.c
	libdisasm/x86_operand_list.c
)
