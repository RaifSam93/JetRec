# $Id: CMakeLists.txt 757302 2016-06-23 15:19:52Z delsart $
################################################################################
# Package: JetRec
################################################################################

# Declare the package name:
atlas_subdir( JetRec )

# Declare the package's dependencies:
atlas_depends_on_subdirs(
  PUBLIC
  Control/AthLinks
  Control/AthToolSupport/AsgTools
  Event/xAOD/xAODCaloEvent
  Event/xAOD/xAODJet
  Event/xAOD/xAODMuon
  GaudiKernel
  Reconstruction/EventShapes/EventShapeInterface
  Reconstruction/Jet/JetEDM
  Reconstruction/Jet/JetInterface
  PRIVATE
  Control/AthenaBaseComps
  Event/xAOD/xAODBase
  Event/xAOD/xAODCore
  Event/xAOD/xAODEventInfo
  Event/xAOD/xAODTracking )

# External dependencies:
find_package( FastJet COMPONENTS fastjetplugins fastjettools siscone
  siscone_spherical )
find_package( FastJetContrib COMPONENTS VariableR )
find_package( ROOT COMPONENTS Core )

# Component(s) in the package:
atlas_add_library( JetRecLib
  JetRec/*.h Root/*.cxx
  PUBLIC_HEADERS JetRec
  INCLUDE_DIRS ${ROOT_INCLUDE_DIRS} ${FASTJET_INCLUDE_DIRS}
  PRIVATE_INCLUDE_DIRS ${FASTJETCONTRIB_INCLUDE_DIRS}
  LINK_LIBRARIES ${ROOT_LIBRARIES} ${FASTJET_LIBRARIES} AthLinks AsgTools
  xAODCaloEvent xAODJet xAODMuon EventShapeInterface JetEDM
  JetInterface
  PRIVATE_LINK_LIBRARIES ${FASTJETCONTRIB_LIBRARIES} xAODBase xAODCore
  xAODEventInfo xAODTracking )

atlas_add_component( JetRec
  src/*.h src/*.cxx src/components/*.cxx
  LINK_LIBRARIES GaudiKernel AthenaBaseComps JetInterface JetRecLib )

# Install files from the package:
atlas_install_python_modules( python/*.py )
atlas_install_joboptions( share/JetRec_jobOptions.py share/myjets.py )

atlas_add_dictionary( JetRecDict
                      JetRec/JetRecDict.h
                      JetRec/selection.xml
                      INCLUDE_DIRS ${ROOT_INCLUDE_DIRS}
                      LINK_LIBRARIES ${ROOT_LIBRARIES} AthLinks AsgTools
                      xAODCaloEvent xAODJet xAODMuon EventShapeInterface JetEDM
                      JetInterface JetRecLib )