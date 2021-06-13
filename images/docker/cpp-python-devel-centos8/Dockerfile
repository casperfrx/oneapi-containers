ARG BASE_IMAGE_DISTRO=centos
ARG BASE_IMAGE_VERSION=8.3.2011

FROM "$BASE_IMAGE_DISTRO":"$BASE_IMAGE_VERSION" AS intel_oneapi_tools

ARG CMAKE_URL=https://github.com/Kitware/CMake/releases/download/v3.20.2/cmake-3.20.2-linux-x86_64.sh
ARG REPO_URL=https://yum.repos.intel.com/oneapi
ARG DISABLE_CERT_CHECK

RUN true \
    &&  dnf install -y \
            make gcc gcc-c++ kernel-devel \
            pkgconfig which bzip2 curl \
            glibc-langpack-en ncurses-compat-libs \
    &&  curl -sSL -o /tmp/cmake.sh "$CMAKE_URL" \
    &&  bash /tmp/cmake.sh --prefix=/usr --skip-license \
    &&  dnf clean all \
    &&  rm -rf \
        /{tmp,var/tmp}/*

ENV LANG=C.UTF-8

RUN true \
    &&  echo -e "\
[oneAPI]\n\
name=Intel(R) oneAPI repository\n\
baseurl=${REPO_URL}\n\
enabled=1\n\
gpgcheck=1\n\
repo_gpgcheck=1\n\
gpgkey=https://yum.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2023.PUB" \
> /etc/yum.repos.d/oneAPI.repo \
    &&  if [ "$DISABLE_CERT_CHECK" ] ; then echo "sslverify=false" >> /etc/yum.conf ; fi

FROM intel_oneapi_tools AS intel_optimized_toolkit_image

RUN true \
    && dnf install -y \
        dnf-plugins-core \
# Intel® oneAPI Common License
        intel-oneapi-common-licensing \
# Intel® oneAPI Common Environment Scripts
        intel-oneapi-common-vars \
# Intel® oneAPI Math Kernel Library
        intel-oneapi-mkl-devel \
# Intel® oneAPI DPC++/C++ Compiler
        intel-oneapi-compiler-dpcpp-cpp \ 
# Intel® Distribution for Python
        intel-oneapi-python \
# Intel® Optimization for TensorFlow
        intel-oneapi-tensorflow \
# Intel® oneAPI Data Analytics Library Development Package
        intel-oneapi-dal-devel \ 
# Intel® Integrated Performance Primitives
        intel-oneapi-ipp-devel \ 
# Intel® Integrated Performance Primitives Cryptography
        intel-oneapi-ippcp-devel \
    &&  dnf clean all \
    &&  rm -rf \
        /{tmp,var/tmp}/*

# setvars.sh environment variables
ENV ACL_BOARD_VENDOR_PATH='/opt/Intel/OpenCLFPGA/oneAPI/Boards' \
    CCL_CONFIGURATION='cpu_gpu_dpcpp' \
    CCL_ROOT='/opt/intel/oneapi/ccl/2021.2.0' \
    CLASSPATH='/opt/intel/oneapi/mpi/2021.2.0//lib/mpi.jar:/opt/intel/oneapi/dal/2021.2.0/lib/onedal.jar' \
    CMAKE_PREFIX_PATH='/opt/intel/oneapi/tbb/2021.2.0/env/..:/opt/intel/oneapi/dal/2021.2.0' \
    CONDA_DEFAULT_ENV='base' \
    CONDA_EXE='/opt/intel/oneapi/intelpython/latest/bin/conda' \
    CONDA_PREFIX='/opt/intel/oneapi/intelpython/latest' \
    CONDA_PROMPT_MODIFIER='(base) ' \
    CONDA_PYTHON_EXE='/opt/intel/oneapi/intelpython/latest/bin/python' \
    CONDA_SHLVL='1'

ENV CPATH='/opt/intel/oneapi/tbb/2021.2.0/env/../include:/opt/intel/oneapi/mpi/2021.2.0//include:/opt/intel/oneapi/mkl/latest/include:/opt/intel/oneapi/ippcp/2021.2.0/include:/opt/intel/oneapi/ipp/2021.2.0/include:/opt/intel/oneapi/dev-utilities/2021.2.0/include:/opt/intel/oneapi/dal/2021.2.0/include:/opt/intel/oneapi/compiler/2021.2.0/linux/include:/opt/intel/oneapi/ccl/2021.2.0/include/cpu_gpu_dpcpp'
ENV DAALROOT='/opt/intel/oneapi/dal/2021.2.0'
ENV DALROOT='/opt/intel/oneapi/dal/2021.2.0'
ENV DAL_MAJOR_BINARY='1'
ENV DAL_MINOR_BINARY='1'
ENV FI_PROVIDER_PATH='/opt/intel/oneapi/mpi/2021.2.0//libfabric/lib/prov:/usr/lib64/libfabric'
ENV INFOPATH='/opt/intel/oneapi/debugger/10.1.1/documentation/info/'
ENV INTELFPGAOCLSDKROOT='/opt/intel/oneapi/compiler/2021.2.0/linux/lib/oclfpga'
ENV INTEL_PYTHONHOME='/opt/intel/oneapi/debugger/10.1.1/dep'
ENV IPPCP_TARGET_ARCH='intel64'
ENV IPPCRYPTOROOT='/opt/intel/oneapi/ippcp/2021.2.0'
ENV IPPROOT='/opt/intel/oneapi/ipp/2021.2.0'
ENV IPP_TARGET_ARCH='intel64'
ENV I_MPI_ROOT='/opt/intel/oneapi/mpi/2021.2.0'
ENV LANG='C.UTF-8'
ENV LD_LIBRARY_PATH='/opt/intel/oneapi/tbb/2021.2.0/env/../lib/intel64/gcc4.8:/opt/intel/oneapi/mpi/2021.2.0//libfabric/lib:/opt/intel/oneapi/mpi/2021.2.0//lib/release:/opt/intel/oneapi/mpi/2021.2.0//lib:/opt/intel/oneapi/mkl/latest/lib/intel64:/opt/intel/oneapi/ippcp/2021.2.0/lib/intel64:/opt/intel/oneapi/debugger/10.1.1/dep/lib:/opt/intel/oneapi/debugger/10.1.1/libipt/intel64/lib:/opt/intel/oneapi/debugger/10.1.1/gdb/intel64/lib:/opt/intel/oneapi/dal/2021.2.0/lib/intel64:/opt/intel/oneapi/compiler/2021.2.0/linux/lib:/opt/intel/oneapi/compiler/2021.2.0/linux/lib/x64:/opt/intel/oneapi/compiler/2021.2.0/linux/lib/emu:/opt/intel/oneapi/compiler/2021.2.0/linux/lib/oclfpga/host/linux64/lib:/opt/intel/oneapi/compiler/2021.2.0/linux/lib/oclfpga/linux64/lib:/opt/intel/oneapi/compiler/2021.2.0/linux/compiler/lib/intel64_lin:/opt/intel/oneapi/compiler/2021.2.0/linux/compiler/lib:/opt/intel/oneapi/ccl/2021.2.0/lib/cpu_gpu_dpcpp'
ENV LIBRARY_PATH='/opt/intel/oneapi/tbb/2021.2.0/env/../lib/intel64/gcc4.8:/opt/intel/oneapi/mpi/2021.2.0//libfabric/lib:/opt/intel/oneapi/mpi/2021.2.0//lib/release:/opt/intel/oneapi/mpi/2021.2.0//lib:/opt/intel/oneapi/mkl/latest/lib/intel64:/opt/intel/oneapi/ippcp/2021.2.0/lib/intel64:/opt/intel/oneapi/ipp/2021.2.0/lib/intel64:/opt/intel/oneapi/dal/2021.2.0/lib/intel64:/opt/intel/oneapi/compiler/2021.2.0/linux/compiler/lib/intel64_lin:/opt/intel/oneapi/compiler/2021.2.0/linux/lib:/opt/intel/oneapi/ccl/2021.2.0/lib/cpu_gpu_dpcpp'
ENV MANPATH='/opt/intel/oneapi/mpi/2021.2.0/man:/opt/intel/oneapi/debugger/10.1.1/documentation/man::/opt/intel/oneapi/compiler/2021.2.0/documentation/en/man/common::'
ENV MKLROOT='/opt/intel/oneapi/mkl/latest'
ENV NLSPATH='/opt/intel/oneapi/mkl/latest/lib/intel64/locale/%l_%t/%N'
ENV OCL_ICD_FILENAMES='libintelocl_emu.so:libalteracl.so:/opt/intel/oneapi/compiler/2021.2.0/linux/lib/x64/libintelocl.so'
ENV ONEAPI_ROOT='/opt/intel/oneapi'
ENV PATH='/opt/intel/oneapi/mpi/2021.2.0//libfabric/bin:/opt/intel/oneapi/mpi/2021.2.0//bin:/opt/intel/oneapi/mkl/latest/bin/intel64:/opt/intel/oneapi/intelpython/latest/bin:/opt/intel/oneapi/intelpython/latest/condabin:/opt/intel/oneapi/dev-utilities/2021.2.0/bin:/opt/intel/oneapi/debugger/10.1.1/gdb/intel64/bin:/opt/intel/oneapi/compiler/2021.2.0/linux/lib/oclfpga/llvm/aocl-bin:/opt/intel/oneapi/compiler/2021.2.0/linux/lib/oclfpga/bin:/opt/intel/oneapi/compiler/2021.2.0/linux/bin/intel64:/opt/intel/oneapi/compiler/2021.2.0/linux/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin'
ENV PKG_CONFIG_PATH='/opt/intel/oneapi/mkl/latest/tools/pkgconfig:'
ENV SETVARS_COMPLETED='1'
ENV SETVARS_VARS_PATH='/opt/intel/oneapi/vtune/latest/env/vars.sh'
ENV TBBROOT='/opt/intel/oneapi/tbb/2021.2.0/env/..'
ENV _CE_CONDA=''
ENV _CE_M=''
