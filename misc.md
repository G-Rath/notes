# Misc notes that are useful but don't have a better place to live (yet)

## How can I find out what my public IP is in a script?

https://checkip.amazonaws.com/

There are a few other highly stable URLs you can use, but this one can look more
professional since you're hitting AWS instead of a random url in your script.

## How can I get `nano` in a `heroku run bash` session?

So you've just done `heroku run bash` to debug your production app, but find you
can't edit any files as there are no editors installed, and getting them
installed seems to be either really hacky or painful.

So do this instead:

```
mkdir /app/nano
curl https://github.com/Ehryk/heroku-nano/raw/master/heroku-nano-2.5.1/nano.tar.gz --location --silent | tar xz -C /app/nano
export PATH=$PATH:/app/nano
```

## How can I get AWS creds setup from just a profile?

Sometimes you're using a thing that requires `AWS_ACCESS_KEY_ID` &
`AWS_SECRET_ACCESS_KEY` to be set, rather than an `AWS_PROFILE`.

This isn't the end of the world, but it is annoying to have to stop what you're
doing to dig up these values and set them up in your env.

Luckily, you can get these values from the `aws` cli with `aws configure`,
meaning you can use the following to handle getting the keys for the profile you
wish to use:

```
export AWS_PROFILE="<profile name>"
export AWS_ACCESS_KEY_ID=$(aws configure get aws_access_key_id)
export AWS_SECRET_ACCESS_KEY=$(aws configure get aws_secret_access_key)
```

# How can I have two versions of `npm`? (or other global `node` packages in general)

Using `nodenv`:

Effectively, you want to create a "alias" for a node version that you install
the alternative version of `npm`. Then you can opt specific projects into using
that version of node using the regular methods, i.e `.node-version` or
`NODENV_VERSION`.

You can do this by navigating to where `nodenv` stores its versions (you can get
this using `nodenv root`), and copying the desired version to a new folder with
a different name.

i.e, to create a version of node 14.16.0 with `npm@7`:

```shell
# 1. install the version of node you want to use
nodenv install 14.16.0

# 2. navigate to where nodenv stores its node versions
cd $(nodenv root)/versions

# 3. copy the version of node you wish to use to another folder with a new name
cp -r 14.16.0{,+npm7}

# 4. install the version of npm you wish to use
NODENV_VERSION=14.16.0+npm7 npm i -g npm@7
```

Now you can have a project use this version of npm via .node-version:

    echo '14.16.0+npm7' > .node-version

# How do I run specific tests with `rspec`?

Say you have the following `context` that you'd like to run, in between a bunch
of other tests:

```ruby
RSpec.describe ContactController, type: :controller do
  # ...

  context "when the submission is spam" do
    before do
      allow(Akismet).to receive(:spam?).and_return(false)
    end

    it "does not create a mailer" do
      expect do
        get :create, params: valid_params
      end.not_to change { ActionMailer::Base.deliveries.count }
    end

    it "returns user to new contact page" do
      get :create, params: valid_params
      expect(response).to render_template :new
    end
  end

  # ...
end
```

You can add a tag to the block, and then have `rspec` run only tests that have
that tag with `--tag`:

```ruby
  # ...

  context "when the submission is spam", :focus do
    # ...
  end

  # ...
```

```shell
bundle exec rspec --tag focus
```

## How do I generate a new ssh-key for a server?

```
ssh-keygen -t rsa -b 4096 -N '' -f ./temp_key
```

## How do I strip a trailing newline from a commands output?

```
tr -d '\n'
```

i.e

```shell
cat ~/.ssh/id_rsa.pub | tr -d '\n'
whats_my_public_ip | tr -d '\n'

# etc...
```

## How can I find duplicate rows in Google Sheets?

Use the following custom script:

```
const ensureArray = input => Array.isArray(input) ? input : [[input]];

const computeHash = str => {
  let rawHash = Utilities.computeDigest(Utilities.DigestAlgorithm.MD5, str);
  let txtHash = '';

  for (i = 0; i < rawHash.length; i++) {
    let hashVal = rawHash[i];

    if (hashVal < 0) {
      hashVal += 256;
    }

    if (hashVal.toString(16).length == 1) {
      txtHash += '0';
    }

    txtHash += hashVal.toString(16);
  }

  return txtHash;
}

function MD5 (input) {
  const rows = ensureArray(input);

  return rows.map(row => computeHash(row.join()));
}
```

Setup a column that selects all the columns in your row:

    =MD5(A82:Q82)

And then add a conditional formatting rule for that column with this "Custom
formula is":

    =countif(A:A,A1)>1

# Why is `binding.pry` stopping in the wrong place? (Usually after editing my code)

Seems to be an issue with
[bootsnap](https://github.com/deivid-rodriguez/byebug/issues/452)

# How do I get `husky` hooks to play nice with WSLv1?

Add a script with the name of the command you'd like to shim in a folder on your
Windows Path

For example, I've got `C:\bin` on my Path for this very purpose.

Use this as the contents of the script:

```
# windows shim of yarn that is called by husky and friends

echo "windows yarn was called"

cmd.exe '/C C:\Users\<user>\AppData\Local\Microsoft\WindowsApps\ubuntu.exe run "/home/<user>/.nodenv/shims/yarn"' $@
```

Replacing `yarn` with the command you want to run

# "failed to initialize build cache" error when setting up Go projects in IntelliJ

> failed to initialize build cache at
> C:\WINDOWS\system32\config\systemprofile\go\cache: mkdir
> C:\WINDOWS\system32\config\systemprofile: Cannot create a file when that file
> already exists.

It seems that IntelliJ defaults to sticking its go cache in that location; you
can fix this by adding `GOCACHE` in the `Environment` setting located at
`File | Settings | Languages & Frameworks | Go | Go Modules`

i.e

> GOCACHE=C:\Users\G-Rath\go\cache
