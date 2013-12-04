android player app is uninstalled still playing



HTTP Live Streaming Issue on Samsung galaxy S4

import java.io.IOException;

import android.app.Activity;
import android.content.Context;
import android.content.Intent;
import android.content.pm.ActivityInfo;
import android.content.res.Configuration;
import android.media.AudioManager;
import android.media.MediaPlayer;
import android.media.MediaPlayer.OnErrorListener;
import android.net.Uri;
import android.os.Bundle;
import android.util.DisplayMetrics;
import android.util.Log;
import android.view.Display;
import android.view.MotionEvent;
import android.view.SurfaceHolder;
import android.view.SurfaceView;
import android.view.View;
import android.view.View.OnClickListener;
import android.view.ViewGroup.LayoutParams;
import android.widget.Button;
import android.widget.FrameLayout;
import android.widget.RelativeLayout;

public class VideoPlayer extends Activity implements SurfaceHolder.Callback,
		MediaPlayer.OnPreparedListener, VideoControllerView.MediaPlayerControl,
		android.media.MediaPlayer.OnErrorListener {

	private static final String TAG = "GPlayer";
	SurfaceView videoSurface;
	MediaPlayer player;
	VideoControllerView controller;
	RelativeLayout progresslayout;
	SurfaceHolder videoHolder;
	
	FrameLayout videoSurfaceContainer;
	Context mcontext;

	@Override
	protected void onStart() {
		// TODO Auto-generated method stub
		super.onStart();
		System.out.println("on start called");
		// progresslayout=(RelativeLayout)findViewById(R.id.progresslayout);
		// progresslayout.setVisibility(View.VISIBLE);
		// player = new MediaPlayer();
		// if(player!=null&&!isPlaying())
		if (player != null) {
			if (!player.isPlaying())
				// player.reset();
				/*
				 * try {
				 * 
				 * player.setAudioStreamType(AudioManager.STREAM_MUSIC);
				 * player.setDataSource(VideoPlayer.this, Uri.parse(VideoURL));
				 * player.prepare();
				 */
				player.start();
			// progresslayout.setVisibility(View.GONE);
		}
		/*
		 * catch (IllegalArgumentException e) {
		 * 
		 * System.out.println("<<<<<<<<<<<<<<001>>>>>>>>>>>>>>");
		 * e.printStackTrace(); } catch (SecurityException e) {
		 * System.out.println("<<<<<<<<<<<<<<002>>>>>>>>>>>>>>");
		 * e.printStackTrace(); } catch (IllegalStateException e) {
		 * System.out.println("<<<<<<<<<<<<<<003>>>>>>>>>>>>>>");
		 * e.printStackTrace(); } catch (IOException e) {
		 * System.out.println("<<<<<<<<<<<<<<004>>>>>>>>>>>>>>");
		 * e.printStackTrace(); } catch (Exception e) { // TODO: handle
		 * exception System.out.println("<<<<<<<<<<<<<<005>>>>>>>>>>>>>>");
		 * e.printStackTrace();
		 * 
		 * 
		 * }
		 */

		// }
	}

	public int getScreenOrientation() {
		int orientation = mcontext.getResources().getConfiguration().orientation;

		return orientation;
	}

	OnClickListener orientationChnageListener = new OnClickListener() {

		@Override
		public void onClick(View v) {

			System.out.println(" Orientation work ");

			if (getScreenOrientation() == 1) {

				setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE);

			} else {
				setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_SENSOR_PORTRAIT);

			}

		}
	};
	Button btnOrientation;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_video_player);

		// setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE);

		videoSurface = (SurfaceView) findViewById(R.id.videoSurface);

		videoSurfaceContainer = (FrameLayout) findViewById(R.id.videoSurfaceContainer);

		btnOrientation = (Button) findViewById(R.id.btnOrientation);

		btnOrientation.setOnClickListener(orientationChnageListener);

		videoHolder = videoSurface.getHolder();
		videoHolder.addCallback(this);
		
		progresslayout = (RelativeLayout) findViewById(R.id.progresslayout);
		progresslayout.setVisibility(View.VISIBLE);
		mcontext = this;

		player = new MediaPlayer();
		controller = new VideoControllerView(this);

		try {

			String VideoURL = "HLS URL ";

			player.setAudioStreamType(AudioManager.STREAM_MUSIC);
			player.setDataSource(this, Uri.parse("	http://184.72.239.149/vod/mp4:BigBuckBunny_115k.mov/playlist.m3u8"));// "http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"
			
		
			player.setOnPreparedListener(this);
			player.prepareAsync();
			player.setOnErrorListener(this);

		} catch (IllegalArgumentException e) {

			System.out.println("<<<<<<<<<<<<<<001>>>>>>>>>>>>>>");
			e.printStackTrace();
		} catch (SecurityException e) {
			System.out.println("<<<<<<<<<<<<<<002>>>>>>>>>>>>>>");
			e.printStackTrace();
		} catch (IllegalStateException e) {
			System.out.println("<<<<<<<<<<<<<<003>>>>>>>>>>>>>>");
			e.printStackTrace();
		} catch (IOException e) {
			System.out.println("<<<<<<<<<<<<<<004>>>>>>>>>>>>>>");
			e.printStackTrace();
		} catch (Exception e) {
			// TODO: handle exception
			System.out.println("<<<<<<<<<<<<<<005>>>>>>>>>>>>>>");
			e.printStackTrace();

		}
	}

	@Override
	public boolean onTouchEvent(MotionEvent event) {

		controller.show(btnOrientation);
		System.out.println("touch event called");
		return false;
	}

	// Implement SurfaceHolder.Callback
	@Override
	public void surfaceChanged(SurfaceHolder holder, int format, int width,
			int height) {

		player.setDisplay(holder);
	}

	@Override
	public void surfaceCreated(SurfaceHolder holder) {
		
		player.setDisplay(holder);

		System.out.println("surface created");
		

	}

	@Override
	public void surfaceDestroyed(SurfaceHolder holder) {

	}

	// End SurfaceHolder.Callback

	/*
	 * private void OnErrorListener() { System.out.println(" On error occur");
	 * 
	 * }
	 */
	// Implement MediaPlayer.OnPreparedListener
	@Override
	public void onPrepared(MediaPlayer mp) {
		System.out.println("on prepared called");
		controller.setMediaPlayer(this);
		controller
				.setAnchorView((FrameLayout) findViewById(R.id.videoSurfaceContainer));
		player.start();

		progresslayout.setVisibility(View.GONE);
	}

	// End MediaPlayer.OnPreparedListener

	// Implement VideoMediaController.MediaPlayerControl
	@Override
	public boolean canPause() {
		return true;
	}

	@Override
	public boolean canSeekBackward() {
		return true;
	}

	@Override
	public boolean canSeekForward() {
		return true;
	}

	@Override
	public int getBufferPercentage() {
		return 0;
	}

	@Override
	public int getCurrentPosition() {
		return player.getCurrentPosition();
	}

	@Override
	public int getDuration() {
		return player.getDuration();
	}

	@Override
	public boolean isPlaying() {
		return player.isPlaying();
	}

	@Override
	public void pause() {
		player.pause();
	}

	@Override
	public void seekTo(int i) {
		player.seekTo(i);
	}

	@Override
	protected void onPause() {
		// TODO Auto-generated method stub
		super.onPause();
		try {
			System.out.println("on pause called");
			if (player != null && player.isPlaying()) {
				System.out.println("Onpause called");
				progresslayout.setVisibility(View.GONE);
				player.pause();

				// player.release();

			}
		} catch (Exception e) {
		}
	}

	/*
	 * @Override protected void onResume() { // TODO Auto-generated method stub
	 * super.onResume(); System.out.println("on resume called");
	 * player.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
	 * public void onPrepared(MediaPlayer mp) { player.start(); } });
	 * player.prepareAsync(); if(player!=null) { if(!player.isPlaying()) {
	 * player.start(); } }
	 * 
	 * }
	 */
	/*
	 * @Override protected void onResume() { // TODO Auto-generated method stub
	 * super.onResume(); System.out.println("on resume called"); try{ if (player
	 * != null&& !player.isPlaying()) {
	 * 
	 * player.start(); } }catch (Exception e) { } }
	 */

	@Override
	protected void onDestroy() {
		// TODO Auto-generated method stub
		super.onDestroy();
		System.out.println("on destroy called");
		if (player != null) {

			player.stop();
			player.release();

		}
	}

	@Override
	public void start() {
		player.start();
	}

	@Override
	public boolean isFullScreen() {

		System.out.println(" Vedio screen changes " + true);
		return fullscreen;
	}

	public static boolean fullscreen = true;

	@Override
	public void toggleFullScreen() {

		System.out.println(" Vedio screen changes ");

		onVideoSizeChanged();

	}

	// End VideoMediaController.MediaPlayerControl

	/*
	 * public void onVideoSizeChanged(MediaPlayer mp, int w, int h) { if (w > 0
	 * && h > 0) { RelativeLayout.LayoutParams l; DisplayMetrics metrics = new
	 * DisplayMetrics();
	 * getWindowManager().getDefaultDisplay().getMetrics(metrics); l = new
	 * RelativeLayout.LayoutParams(metrics.heightPixels, metrics.widthPixels);
	 * l.addRule(RelativeLayout.CENTER_IN_PARENT, -1); float scale =
	 * (metrics.heightPixels * 1.0f) / (metrics.widthPixels * 1.0f);
	 * videoView.setScaleX(scale); videoView.setLayoutParams(l);
	 * Log.d(HangWithApp.TAG, "video size: " + mp.getVideoWidth() + "x" +
	 * mp.getVideoHeight() + " vertical scale: " + scale); } }
	 */

	public void onVideoSizeChanged() {

		if (fullscreen) {

			int width = videoSurface.getWidth();
			int height = videoSurface.getHeight();

			float boxWidth = width;
			float boxHeight = height;

			float videoWidth = player.getVideoWidth();
			float videoHeight = player.getVideoHeight();

			// Log.i(TAG,
			// String.format("startVideoPlayback @ %d - video %dx%d - box %dx%d",
			// mPos, (int) videoWidth, (int) videoHeight, width, height));

			float wr = boxWidth / videoWidth / 2;
			float hr = boxHeight / videoHeight / 2;
			float ar = videoWidth / videoHeight / 2;

			System.out.println(" width: " + width + " height : " + height
					+ " wr " + wr + " hr " + hr + " ar " + ar);

			if (getScreenOrientation() == 1) {

				if (width > height)
					width = (int) (boxHeight);
				else
					height = (int) (boxWidth);

			} else {

				if (width > height)
					width = (int) (boxHeight) / 2;
				else
					height = (int) (boxWidth);
			}

			Log.i(TAG, String.format("Scaled to %dx%d", width, height));

			// videoSurface.setLayoutParams(new LayoutParams(width,height));

			videoHolder.setFixedSize(width / 2, height / 2);

			fullscreen = false;
		} else {
			fullscreen = true;

			// RelativeLayout.LayoutParams l;
			// DisplayMetrics metrics = new DisplayMetrics();
			// getWindowManager().getDefaultDisplay().getMetrics(metrics);
			//
			// l = new RelativeLayout.LayoutParams(metrics.heightPixels,
			// metrics.widthPixels);
			// l.addRule(RelativeLayout.CENTER_IN_PARENT, -1);
			// float scale = (metrics.heightPixels * 1.0f) /
			// (metrics.widthPixels * 1.0f);

			// videoHolder.setFixedSize(metrics.heightPixels,
			// metrics.widthPixels);

			System.out.println(" height " + videoSurfaceContainer.getHeight());

			System.out.println(" width " + videoSurfaceContainer.getWidth());

			videoHolder.setFixedSize(videoSurfaceContainer.getWidth(),
					videoSurfaceContainer.getHeight());

			// videoSurface.setLayoutParams(params);

			// videoHolder.setSizeFromLayout();

			// videoView.setScaleX(scale);
			// videoView.setLayoutParams(l);

		}

	}
@Override
protected void onResume() {
	// TODO Auto-generated method stub
	super.onResume();
	if (player != null) {
		
		
		
		
		videoHolder = videoSurface.getHolder();
		
			player.start();
			//player.setDisplay(videoHolder);
		}
			
	System.out.println("on resume called");
}
	@Override
	public boolean onError(MediaPlayer mp, int what, int extra) {

		System.out.println("On error sssssss");

		//finish();

		return false;
	}

	@Override
	public void onBackPressed() {
		Intent myIntent = new Intent(VideoPlayer.this, Watch.class);
		startActivity(myIntent);
		finish();
		overridePendingTransition(0, 0);

	}

}
