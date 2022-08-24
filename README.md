# Lexoffice Voucher Upload

Upload your vouchers/invoices from email attachements to Lexoffice.

## Requirements
- See `requirements.txt`
- pycurl: The installation of pycurl can give you a hard time. Here is what worked for me on Windows, MacOS and Ubuntu.
    - Windows: If installation fails you can try to install from the [unofficial binary](https://www.lfd.uci.e~du/~gohlke/pythonlibs/#pycurl), see this  [stackoverflow comment](https://stackoverflow.com/a/53598619/6679493) for more details)
    - MacOS: Run the following
        ```bash
        brew install openssl
        export PYCURL_SSL_LIBRARY=openssl
        export LDFLAGS="-L/opt/homebrew/opt/openssl@3/lib"
        export CPPFLAGS="-I/opt/homebrew/opt/openssl@3/include"
        pip3 install --no-cache-dir --ignore-installed --compile --install-option="--with-openssl" pycurl
        ```
    - Linux (Ubuntu):
        1. Install requirements:
            ```bash
            apt install libssl-dev libcurl4-openssl-dev libcurl4-gnutls-dev libgnutls-dev python3-dev
            ```
            If you get the error Package *libgnutls-dev* is not available, then instead of `libgnutls-dev` install `libgnutls28-dev`
            
            If this also doesn't work (or `apt` says you have unmet dependencies), try to install everything with `aptitude` instead of `apt`:
            ```bash
            aptitude install libssl-dev libcurl4-openssl-dev libcurl4-gnutls-dev python3-dev
            ```

        2. Then install `pycurl`:
            ```bash
            pip3 install pycurl
            ```

## Usage
1. Install requirements `pip install -r requirements.txt`
2. Specify your configuration in `config.ini` (you can generate a config file with `python3 main.py --generate` or `python3 main.py --generate --config /my/destination/mycustomconfig.conf`)
3. Run `python3 main.py`
4. Mails in specified maildir will automatically be searched for attachements with the configured file extension, then downloaed und uploaded to Lexoffice via their API.

### CLI Arguments
- `-h`, `--help` show the help message
- `-c FILE`, `--config FILE` specify the config file to use. If nothing is specified, `./config.ini` will be used.
- `-q`, `--quiet` don't print status messages to stdout.
- `-g`, `--generate` generate a new configruation file, optionally specify path and filename with `--config` argument.
- `-l`, `--loop`, `--continuous` specify the intervall in seconds between each run. Only takes effect in loop/continuous mode. Default is 120 seconds
- `-i SECONDS`, `--intervall SECONDS` specify the intervall in seconds between each run. Only takes effect in loop/continuous mode. Default is 120 seconds.

### Multiple config files

If you have more than one mailbox to check, you can create multiple config files and iterate/loop over them like with a simple bash script:
```bash
# Generate config files from template with specific destination and file name
python3 main.py --generate --config /path/to/config/config_tom.ini
python3 main.py --generate --config /path/to/config/config_lisa.ini
python3 main.py --generate --config /path/to/config/config_joe.ini

# Run program with specific configuration files
for configfile in /path/to/config/config*.ini; do
    python3 main.py --config $configfile >> logfile.log
done
```