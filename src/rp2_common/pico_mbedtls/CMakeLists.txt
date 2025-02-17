if (PICO_MBEDTLS_PATH)
    message("Using PICO_MBEDTLS_PATH ('${PICO_MBEDTLS_PATH}')")
elseif (DEFINED ENV{PICO_MBEDTLS_PATH} AND (NOT PICO_MBEDTLS_PATH))
    set(PICO_MBEDTLS_PATH $ENV{PICO_MBEDTLS_PATH})
    message("Using PICO_MBEDTLS_PATH from environment ('${PICO_MBEDTLS_PATH}')")
endif()

if (PICO_MBEDTLS_PATH)

    set(src_crypto
        aes.c
        aesni.c
        aria.c
        asn1parse.c
        asn1write.c
        base64.c
        bignum.c
        camellia.c
        ccm.c
        chacha20.c
        chachapoly.c
        cipher.c
        cipher_wrap.c
        constant_time.c
        cmac.c
        ctr_drbg.c
        des.c
        dhm.c
        ecdh.c
        ecdsa.c
        ecjpake.c
        ecp.c
        ecp_curves.c
        entropy.c
        entropy_poll.c
        error.c
        gcm.c
        hkdf.c
        hmac_drbg.c
        md.c
        md5.c
        memory_buffer_alloc.c
        mps_reader.c
        mps_trace.c
        nist_kw.c
        oid.c
        padlock.c
        pem.c
        pk.c
        pk_wrap.c
        pkcs12.c
        pkcs5.c
        pkparse.c
        pkwrite.c
        platform.c
        platform_util.c
        poly1305.c
        psa_crypto.c
        psa_crypto_aead.c
        psa_crypto_cipher.c
        psa_crypto_client.c
        psa_crypto_driver_wrappers.c
        psa_crypto_ecp.c
        psa_crypto_hash.c
        psa_crypto_mac.c
        psa_crypto_rsa.c
        psa_crypto_se.c
        psa_crypto_slot_management.c
        psa_crypto_storage.c
        psa_its_file.c
        ripemd160.c
        rsa.c
        sha1.c
        sha256.c
        sha512.c
        threading.c
        timing.c
        version.c
        version_features.c
    )

    set(src_x509
        x509.c
        x509_create.c
        x509_crl.c
        x509_crt.c
        x509_csr.c
        x509write_crt.c
        x509write_csr.c
    )

    set(src_tls
        debug.c
        net_sockets.c
        ssl_cache.c
        ssl_ciphersuites.c
        ssl_client.c
        ssl_tls12_client.c
        ssl_cookie.c
        ssl_msg.c
        ssl_tls12_server.c
        ssl_ticket.c
        ssl_tls.c
        ssl_tls13_keys.c
    )

    set(MBEDTLS_SRC "")
    foreach(FILE IN LISTS src_crypto src_x509 src_tls)
        list(APPEND MBEDTLS_SRC ${PICO_MBEDTLS_PATH}/library/${FILE})
    endforeach()

    add_library(pico_mbedtls INTERFACE)
    target_sources(pico_mbedtls INTERFACE ${MBEDTLS_SRC})
    target_compile_definitions(pico_mbedtls INTERFACE MBEDTLS_CONFIG_FILE="mbedtls_config.h")
    target_include_directories(pico_mbedtls INTERFACE ${PICO_MBEDTLS_PATH}/include/ ${PICO_MBEDTLS_PATH}/library/)

endif()

