############################################################################
#
#   Copyright (c) 2015-2022 PX4 Development Team. All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
# 3. Neither the name PX4 nor the names of its contributors may be
#    used to endorse or promote products derived from this software
#    without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS
# OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
############################################################################

px4_add_module(
	MODULE modules__simulation__simulator_mavlink
	MAIN simulator_mavlink
	COMPILE_FLAGS
		-Wno-double-promotion
		-Wno-cast-align
		-Wno-address-of-packed-member # TODO: fix in c_library_v2
	INCLUDES
		${CMAKE_BINARY_DIR}/mavlink
		${CMAKE_BINARY_DIR}/mavlink/development
		${CMAKE_BINARY_DIR}/mavlink/common
		${CMAKE_BINARY_DIR}/mavlink/standard
	SRCS
		SimulatorMavlink.cpp
		SimulatorMavlink.hpp
	DEPENDS
		mavlink_c_generate
		conversion
		geo
		drivers_accelerometer
		drivers_gyroscope
		drivers_magnetometer
	)

include(sitl_targets_flightgear.cmake)
include(sitl_targets_gazebo-classic.cmake)
include(sitl_targets_jmavsim.cmake)
include(sitl_targets_jsbsim.cmake)


# optimAeroF16 (legacy compatibility launch helper)
add_custom_target(optimAeroF16
	COMMAND ${CMAKE_COMMAND} -E env PX4_SYS_AUTOSTART=10020 $<TARGET_FILE:px4>
	WORKING_DIRECTORY ${SITL_WORKING_DIR}
	USES_TERMINAL
	DEPENDS
		px4
		${PX4_SOURCE_DIR}/ROMFS/px4fmu_common/init.d-posix/airframes/10020_optimAeroF16
	COMMENT "launching px4 optimAeroF16 (SYS_AUTOSTART=10020)"
)

# optimAeroHex (legacy compatibility launch helper)
add_custom_target(optimAeroHex
	COMMAND ${CMAKE_COMMAND} -E env PX4_SYS_AUTOSTART=10021 $<TARGET_FILE:px4>
	WORKING_DIRECTORY ${SITL_WORKING_DIR}
	USES_TERMINAL
	DEPENDS
		px4
		${PX4_SOURCE_DIR}/ROMFS/px4fmu_common/init.d-posix/airframes/10021_optimAeroHex
	COMMENT "launching px4 optimAeroHex (SYS_AUTOSTART=10021)"
)

function(add_optim_aero_target target_name autostart_id)
	add_custom_target(${target_name}
		COMMAND ${CMAKE_COMMAND} -E env PX4_SYS_AUTOSTART=${autostart_id} $<TARGET_FILE:px4>
		WORKING_DIRECTORY ${SITL_WORKING_DIR}
		USES_TERMINAL
		DEPENDS
			px4
			${PX4_SOURCE_DIR}/ROMFS/px4fmu_common/init.d-posix/airframes/${autostart_id}_${target_name}
		COMMENT "launching px4 ${target_name} (SYS_AUTOSTART=${autostart_id})"
	)
endfunction()

add_optim_aero_target(optimAeroQuad 10022)
add_optim_aero_target(optimAeroAckermann 10023)
add_optim_aero_target(optimAeroOcta 10024)
add_optim_aero_target(optimAeroEVTOL 10025)
add_optim_aero_target(optimAeroDiffRover 10026)
add_optim_aero_target(optimAeroTracked 10027)
add_optim_aero_target(optimAeroLoader 10028)
add_optim_aero_target(optimAeroUSV 10029)
add_optim_aero_target(optimAeroUUV 10033)
add_optim_aero_target(optimAeroBicycle 10034)
add_optim_aero_target(optimAeroUnicycle 10035)
add_optim_aero_target(optimAeroFixedwingPlane 10036)
add_optim_aero_target(optimAeroVtolTailsitter 10037)
add_optim_aero_target(optimAeroShipSurface 10038)
add_optim_aero_target(optimAeroNpsAuv 10039)
add_optim_aero_target(optimAeroDsrv 10040)
add_optim_aero_target(optimAeroSemisub 10041)
add_optim_aero_target(optimAeroC172p 10043)
add_optim_aero_target(optimAeroC172pJSBSim 10044)
