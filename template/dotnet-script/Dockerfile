FROM openfaas/classic-watchdog:0.18.13 as watchdog

FROM mcr.microsoft.com/dotnet/core/sdk:3.1

ENV DOTNET_CLI_TELEMETRY_OPTOUT 1

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

# Create a non-root user
RUN addgroup --system app \
    && adduser --system --ingroup app app
    
WORKDIR /home/app/

COPY function .
RUN chown app:app -R /home/app
    
USER app

ENV fprocess="dotnet script ./handler.csx"
EXPOSE 8080

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]