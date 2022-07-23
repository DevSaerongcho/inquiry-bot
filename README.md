<h1 align="center">리아 네트워크 문의 봇 구동 방식</h1>

 - 서버 보안과, 안전을 위해 index 코드만 올리고 나머지는 사진으로 대체 하겠습니다.
 - 사진은, 소스 코드가 아닌, 작동 후 작업 완료한 사진 들 입니다.

<br>


- index.js

```c
const fs = require('fs');
const {
  Client,
  Collection,
  Intents
} = require('discord.js');
const chalk = require('chalk')
const config = require('./config.json');

const client = new Client({
  intents: [Intents.FLAGS.GUILDS, Intents.FLAGS.GUILD_MESSAGES, Intents.FLAGS.GUILD_MEMBERS],
});

const Discord = require('discord.js');
client.discord = Discord;
client.config = config;

client.on("ready", () => {
  console.log(`Logged in as ${client.user.tag}!`)
  client.user.setActivity(`리아 네트워크`, { type: "STREAMING" })

  
})

client.commands = new Collection();
const commandFiles = fs.readdirSync('./commands').filter(file => file.endsWith('.js'));

for (const file of commandFiles) {
  const command = require(`./commands/${file}`);
  client.commands.set(command.data.name, command);
};

const eventFiles = fs.readdirSync('./events').filter(file => file.endsWith('.js'));

for (const file of eventFiles) {
  const event = require(`./events/${file}`);
    client.on(event.name, (...args) => event.execute(...args, client));
};

client.on('interactionCreate', async interaction => {
  if (!interaction.isCommand()) return;

  const command = client.commands.get(interaction.commandName);
  if (!command) return;

  try {
    await command.execute(interaction, client, config);
  } catch (error) {
    console.error(error);
    return interaction.reply({
      content: 'There was an error while executing this command!',
      ephemeral: true
    });
  };
});

client.login(require('./config.json').token);
```
<br>
- 문의 시스템
<br>
<img src="https://cdn.discordapp.com/attachments/998559272155750482/1000362304266846318/01.PNG" />
<br>
<img src="https://cdn.discordapp.com/attachments/998559272155750482/1000362304552050708/03.PNG" />
<br>
<img src="https://cdn.discordapp.com/attachments/998559272155750482/1000362304870813706/04.PNG" />
<br>
<img src="https://cdn.discordapp.com/attachments/998559272155750482/1000362305286066176/05.PNG" />
<br>
<img src="https://cdn.discordapp.com/attachments/998559272155750482/1000362303977422928/06.PNG" />
