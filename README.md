minecraft_bot for MC 1.5.6
==========================
Everything needed to update the bot is to update the sections listed below:


-------------------------------
Minecraft.java
-------------------------------

--->PLACER UN APPEL � 'MinecraftBot.run()' � L'ENDROIT APPROPRI� */
	
	E.G.:
	
		import net.minecraft.src.MinecraftBot;
		
		public MinecraftBot mBot;
		mBot = new MinecraftBot(this);
		
		public void runTick()
			mBot.run();
			
	E.G.:
	
		public void startGame() throws LWJGLException
			mBot.loadSettings();
		
--->ANNULER LES APPELS CONSTANTS � 'playerController.resetBlockRemoving()' LORSQUE LE BOT EST EN FONCTION 
	DANS LA M�THODE 'sendClickBlockToController(int par1, boolean par2)' */
	
	E.G.:
	
		else if(!mBot.isActive())
            {
                this.playerController.resetBlockRemoving();
            }
			
	E.G.:
		
		public void setIngameFocus()
		{
			if (Display.isActive() && mBot.inGameHasFocus)
		
		
-------------------------------
EntityClientPlayerMP.java
-------------------------------

--->MODIFIER 'sendChatMessage(String par1Str)' POUR QUE LA M�THODE RENVOIE LES MESSAGES RE�U VERS 'MinecraftBot.mbChat.sendChatMessage(s)' */
	
	E.G.:
	
		public void sendChatMessage(String par1Str)
		{
			mc.mBot.mbChat.sendChatMessage(par1Str);
		}
		
		
-------------------------------
NetClientHandler.java
-------------------------------

	E.G.:

		public void handleUpdateTime(Packet4UpdateTime par1Packet4UpdateTime)
		{
			this.mc.mBot.setWorldTime(par1Packet4UpdateTime.time);
		}
		
		
-------------------------------
EntityRenderer.java
-------------------------------

--->ANNULER LA PAUSE AUTOMATIQUE LORSQUE L'�CRAN PERD LE FOCUS */
	
	E.G.:
	
		public void updateCameraAndRender(float par1)
		
			if (Minecraft.getSystemTime() - this.prevFrameTime > 500L && !mc.mBot.isActive())
            {
                this.mc.displayInGameMenu();
            }
		
		
-------------------------------
EnumOptions.java
-------------------------------

	E.G.:
	
		LIGHTNING("Keep light over", false, false);

		
-------------------------------
GameSettings.java
-------------------------------

--->AJOUTER LES TOUCHES
	
	E.G.:
	
		public KeyBinding keyBindBotPause = new KeyBinding("Bot Pause", 45);
		public KeyBinding keyBindBotMenu = new KeyBinding("Bot Menu", 46);
		public KeyBinding keyBindBotMacro = new KeyBinding("Bot Macro", 47);
		
--->AJOUTER " this.keyBindBotMenu, this.keyBindBotPause, this.keyBindBotMacro," DANS LES DEUX �NUM�RATIONS
		
--->AJOUTER LES VARIABLES:
	
	E.G.:
	
		/** MinecraftBot options */
		public float lightning = 0.25f;
		public boolean torch = true;
		public boolean left;
		public String macro = "sq:3,3,2";
		public int itemId = 54;
		
	E.G.:
		
		public float getOptionFloatValue(EnumOptions par1EnumOptions)
		{
			return par1EnumOptions == EnumOptions.FOV ? this.fovSetting : (par1EnumOptions == EnumOptions.GAMMA ? this.gammaSetting : (par1EnumOptions == EnumOptions.MUSIC ? this.musicVolume : (par1EnumOptions == EnumOptions.SOUND ? this.soundVolume : (par1EnumOptions == EnumOptions.SENSITIVITY ? this.mouseSensitivity : (par1EnumOptions == EnumOptions.CLOUD_HEIGHT ? this.ofCloudsHeight : (par1EnumOptions == EnumOptions.AO_LEVEL ? this.ofAoLevel : (par1EnumOptions == EnumOptions.RENDER_DISTANCE_FINE ? (float)(this.ofRenderDistanceFine - 32) / 480.0F : (par1EnumOptions == EnumOptions.CHAT_OPACITY ? this.chatOpacity : (par1EnumOptions == EnumOptions.LIGHTNING ? this.lightning : 0.0F)))))))));
		}
		
	E.G.:
	
		public void setOptionFloatValue(EnumOptions par1EnumOptions, float par2)
		{
			if (par1EnumOptions == EnumOptions.LIGHTNING)
			{
				this.lightning = par2;
			}
		
	E.G.:
	
		public String getKeyBinding(EnumOptions par1EnumOptions)
		{
			...
			
			return par1EnumOptions == EnumOptions.RENDER_DISTANCE ? var3 + getTranslation(RENDER_DISTANCES, this.renderDistance) : (par1EnumOptions == EnumOptions.DIFFICULTY ? var3 + getTranslation(DIFFICULTIES, this.difficulty) : (par1EnumOptions == EnumOptions.GUI_SCALE ? var3 + getTranslation(GUISCALES, this.guiScale) : (par1EnumOptions == EnumOptions.CHAT_VISIBILITY ? var3 + getTranslation(CHAT_VISIBILITIES, this.chatVisibility) : (par1EnumOptions == EnumOptions.PARTICLES ? var3 + getTranslation(PARTICLES, this.particleSetting) : (par1EnumOptions == EnumOptions.FRAMERATE_LIMIT ? var3 + getTranslation(LIMIT_FRAMERATES, this.limitFramerate) : (par1EnumOptions == EnumOptions.GRAPHICS ? (this.fancyGraphics ? var3 + var2.translateKey("options.graphics.fancy") : var3 + var2.translateKey("options.graphics.fast")) : (par1EnumOptions == EnumOptions.LIGHTNING ? var3 + (int)(this.lightning * 100.0F) + "%" : var3)))))));
	
		
-------------------------------
GuiControls.java
-------------------------------

--->MODIFIER L'ESPACEMENT ENTRE LES BOUTONS AINSI QUE L'ESPACEMENT ENTRE LES NOMS DE BOUTONS
	
	E.G.
	
		for (int var3 = 0; var3 < this.options.keyBindings.length; ++var3)
			{
				this.controlList.add(new GuiSmallButton(var3, var2 + var3 % 2 * 160, this.height / 8 + 20 * (var3 >> 1), 70, 20, this.options.getOptionDisplayString(var3)));
			}

			this.controlList.add(new GuiButton(200, this.width / 2 - 100, this.height / 6 + 178, var1.translateKey("gui.done")));
		
	
-------------------------------
PlayerControllerMP.java
-------------------------------

--->"if(!mc.mBot.isActive()) this.onPlayerDestroyBlock(par1, par2, par3, par4)" DANS "public void clickBlock(int par1, int par2, int par3, int par4)"
	ET DANS "public void onPlayerDamageBlock(int par1, int par2, int par3, int par4)"
		
		
-------------------------------
MBGuiContainerCreative.java
-------------------------------

(This class is basically a copy of 'GuiContainerCreative' in which I change every call to 'isInCreativeMode' to be true.)

--->isInCreativeMode thing and return value of handleMouseClick

	E.G.
	
		if (this.inventorySlots.getInventory().get(par1Slot.slotNumber)!=null) mc.displayGuiScreen(new MBGuiMenu((ItemStack)this.inventorySlots.getInventory().get(par1Slot.slotNumber), mc));
		
		**INSTEAD OF**
		
		this.inventorySlots.slotClick(par1Slot.slotNumber, par3, par4, this.mc.thePlayer);
        ItemStack var11 = this.inventorySlots.getSlot(par1Slot.slotNumber).getStack();
        this.mc.playerController.sendSlotPacket(var11, par1Slot.slotNumber - this.inventorySlots.inventorySlots.size() + 9 + 36);
