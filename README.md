UnitTestSnippet
===============

A set of VisualStudio code snippet for some unit test case


Testing A PRISM EventAggregator Event
=====================================

This is the first snippet. 
Consider this scenario: we are in an MVVM WPF/WP7 application and we are using the PRISM library. In particular we are using EventAggregator as our message broker. We want to test the code of a method which subscribe an event. Someting like this :

<pre lang="csharp">
public class ViewModel
{
  private bool _isEnabled;
 
  public bool IsEnabled
  {
     get { return _isEnabled; }
  }
 
  public ViewModel(IEventAggregator eventAggregator)
  {
     eventAggregator.GetEvent<EnableFlagEvent>().Subscribe(OnEnableFlagEvent, ThreadOption.PublisherThread, true);
  }
 
  private void OnEnableFlagEvent(EventArgs obj)
  {
     _isEnabled = true;
  }
}
</pre>

